# FIX WEBHOOK — LID NORMALIZATION + 9º DÍGITO BR

> Fix cirúrgico no whatsapp-webhook: resolve LID-only + normaliza 9º dígito BR.
> 1 arquivo, 1 linha alterada, 1 deploy.

---

<contexto>
Smoke test Fernando Vaz passou em deploy. Greeting sai. Mas respostas do
André no zap NÃO chegam no banco apesar de ✓✓ entregues. Diagnóstico em prod
via 2 curls:

**DIAG_TEST_1** — payload LID-only + remoteJidAlt
```json
{
  "key": {
    "remoteJid": "70322187427935@lid",
    "remoteJidAlt": "558496967963@s.whatsapp.net"
  }
}
```
Resultado: HTTP 200 fall-through em 1.59s. Handler não invocado. Zero rows.

**DIAG_TEST_2** — payload 13 dígitos + @s.whatsapp.net
```json
{
  "key": { "remoteJid": "5584996967963@s.whatsapp.net" }
}
```
Resultado: HTTP 200 handled_by fernando-vaz. 9.95s. Fernando Vaz respondeu.
Row criada em whatsapp_conversations. Pipeline completa funciona.

**Cenário B confirmado:** Fix LID anterior em prod faz só
`.replace('@s.whatsapp.net', '')`. Não limpa `@lid`, não usa remoteJidAlt.
Quando Evolution entrega LID-only (WhatsApp Meta nova verba), webhook silencia.

**Bug secundário** — mismatch 9º dígito:
- remoteJidAlt do André = 558496967963@s.whatsapp.net (12 dígitos, sem 9)
- whatsapp_number do tenant = 5584996967963 (13 dígitos, com 9)
- Só trocar rawJid → altJid não resolve, ainda dá mismatch

**Direção de normalização:** adicionar 9 no input (não remover do banco).
Banco armazena 13 dígitos = formato BR moderno correto. Mais seguro
normalizar entrada.

**Side effect aceito:** número 558491671407 (Nocast antigo / sogra, 12 dígitos)
também será normalizado pra 5584991671407. Nocast em transição, não afeta.

**Cleanup prévio já feito via MCP:** lixo do DIAG_TEST_2 removido de
whatsapp_conversations. Tenant 94362267 voltou ao estado virgem (1 row out).
</contexto>

<escopo>
  <arquivos_permitidos>
    supabase/functions/whatsapp-webhook/index.ts
  </arquivos_permitidos>
  <arquivos_proibidos>
    supabase/functions/fernando-vaz-start/index.ts
    supabase/functions/fernando-vaz/index.ts
    supabase/functions/_shared/*
    schema brand_identity, tenants, whatsapp_conversations
    tenant Nocast (de274ffb-abe0-41de-9f4e-7c9ac49a68a4)
    tenant "Andre Test FV" (preservado pra análise)
    tenant "Andre Test FV Pós-Fix" (94362267-d462-4a47-9f23-62f5c9537771)
    agent_semantic_memory, agent_procedural_memory
  </arquivos_proibidos>
</escopo>

<tarefa>

# PASSO 1 — Localizar código atual

Abrir `supabase/functions/whatsapp-webhook/index.ts` na linha ~182-192.
Procurar linha que calcula variável `from`. Provável forma atual:

```typescript
const from = body?.data?.key?.remoteJid?.replace('@s.whatsapp.net', '') || '';
```

(Pode estar em linha próxima a 182 dependendo da versão atual baixada.)

# PASSO 2 — Substituir por snippet completo

Trocar a linha única de cálculo de `from` por este bloco:

```typescript
// Resolver phone com LID fallback + normalização BR 9º dígito
const rawJid = body?.data?.key?.remoteJid || '';
const altJid = body?.data?.key?.remoteJidAlt || '';
const jidSource = rawJid.endsWith('@lid') && altJid.endsWith('@s.whatsapp.net')
  ? altJid
  : rawJid;
let from = jidSource.replace(/@(s\.whatsapp\.net|lid|c\.us)$/, '');

// Normalizar 9º dígito BR: 55 + DDD(2) + 8 dígitos → injeta 9
// Só afeta números BR de 12 dígitos. Números 13+ não são alterados.
if (/^55\d{10}$/.test(from)) {
  from = from.slice(0, 4) + '9' + from.slice(4);
}
```

**Lógica:**
1. Se `remoteJid` termina em `@lid` E `remoteJidAlt` termina em `@s.whatsapp.net`, usa o alt. Senão usa o rawJid.
2. Strip qualquer sufixo (@s.whatsapp.net, @lid, @c.us).
3. Se resultar em 12 dígitos começando com 55, injeta 9 após o DDD.

# PASSO 3 — Deploy

```bash
cd ~/socialagent-fv
supabase functions deploy whatsapp-webhook --no-verify-jwt
```

Esperar saída:
- Deployed Function: whatsapp-webhook
- Bundle size: ~X MB
- Version: incrementado
- Status: ACTIVE

# PASSO 4 — Parar aqui

NÃO rodar curl de teste. NÃO criar tenant novo. NÃO validar por SQL.
André vai mandar msg real no zap e Claude.ai consulta banco pra validar.

</tarefa>

<nao_fazer>
- NÃO alterar lógica do handler fernando-vaz (resto do webhook)
- NÃO alterar filtro Claudionor (CLAUDIONOR_PHONE etc)
- NÃO tocar em sendWhatsApp helper
- NÃO mexer em constantes CLAUDIONOR_PHONE, WHATSAPP_NUMBER
- NÃO mexer em tenants Nocast, Andre Test FV, 94362267
- NÃO criar tenant novo
- NÃO alterar schema
- NÃO redeployar fernando-vaz ou fernando-vaz-start
- NÃO rodar curl de validação
- NÃO limpar whatsapp_conversations ou qualquer tabela
</nao_fazer>

<verificacao>
Pré-deploy:
- Confirmar arquivo baixado reflete estado atual de prod:
  grep -n "remoteJid" supabase/functions/whatsapp-webhook/index.ts

Pós-deploy (responsabilidade Claude.ai via MCP):
- André manda msg real no zap
- Claude.ai consulta:
  SELECT direction, LEFT(message,60), phone, created_at 
  FROM whatsapp_conversations 
  WHERE phone='5584996967963' 
    AND created_at > now() - interval '3 minutes' 
  ORDER BY created_at DESC;
- Esperado: row direction='in' existe
</verificacao>

<criterio_sucesso>
1. ✓ Deploy completou sem erro
2. ✓ Versão nova ACTIVE
3. ✓ Orion reporta versão + tamanho bundle
4. ✓ Nenhuma alteração em outros arquivos
5. ✓ Tenant Nocast intacto (mesmo sem teste, não deve ter sido tocado)
</criterio_sucesso>

<rollback>
```bash
cd ~/socialagent-fv
git log --oneline supabase/functions/whatsapp-webhook/index.ts | head -5
git checkout <hash_anterior> -- supabase/functions/whatsapp-webhook/index.ts
supabase functions deploy whatsapp-webhook --no-verify-jwt
```

Se git não tem histórico (porque download foi manual), reverter manualmente:
- Editar arquivo trocando o snippet novo pela linha única original:
  ```typescript
  const from = body?.data?.key?.remoteJid?.replace('@s.whatsapp.net', '') || '';
  ```
- Re-deploy.
</rollback>

<apos_concluir>
Após deploy confirmado:

1. Reportar ao André:
   - Nova versão (v5 esperado)
   - Bundle size
   - Zero warnings

2. André manda msg real no zap pro número Neotech.

3. Claude.ai consulta banco via MCP em ~10s:
   - Se row in existe + handler invocou fernando-vaz → fix OK
   - Se row in NÃO existe → bug persiste, abrir próximo diagnóstico

4. Se conversa fluir 5+ turnos e brand_identity começar a receber updates,
   tag git: STABLE-WEBHOOK-LID-NORMALIZATION-2026-04-23
</apos_concluir>
