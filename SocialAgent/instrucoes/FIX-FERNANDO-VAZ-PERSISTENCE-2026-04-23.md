# FIX FERNANDO VAZ — PERSISTÊNCIA + BRAND_IDENTITY LIMPO

> 3 bugs descobertos em smoke test real da Neotech via WhatsApp.
> Conversa rica, banco vazio. Fix cirúrgico em 2 arquivos.
> Preserva tenant de teste pra análise antes/depois.

---

<contexto>
Smoke test end-to-end da Neotech rodou. 11 mensagens trocadas. Fernando Vaz
respondeu com qualidade (classificou kit=agencia, manteve contexto, tom bom).
Mas o banco revelou 3 bugs de persistência.

**Bug 1 — fernando-vaz-start NÃO cria row em brand_identity.**
Primeiro UPDATE do main handler em brand_identity afeta 0 rows (silencioso).
Tabela mantém defaults Nocast perpetuamente para o tenant novo.

**Bug 2 — System prompt não documenta nomes reais das colunas.**
Campos portal_niche, portal_tone, portal_differencial, logo_initial,
logo_text_a, logo_text_b, label_breaking_a/b, cta_see_more/carousel
não aparecem em nenhum lugar do código ou prompt. LLM nunca é instruído a
preencher. Só portal_tone escapou porque o LLM adivinhou o nome.

**Bug 3 — Defaults da tabela brand_identity são Nocast hardcoded.**
Preto + dourado + Anton + "NO/CAST" + "BREAKING/NEWS" nascem em todo tenant.

Evidência — tenant "Andre Test FV" (preservar, NÃO deletar ainda):
- portal_tone SIM atualizou ("técnico e acessível, sem buzzword")
- portal_niche, portal_differencial ficaram NULL
- color_primary, color_secondary ficaram com defaults Nocast
- logo_initial, logo_text_a/b idem
- onboarding_step ficou 0 (LLM nunca chamou action=advance_step)

Objetivo: 2 patches cirúrgicos + redeploy. Preservar "Andre Test FV"
pra comparar antes/depois. Criar "Andre Test FV Pós-Fix" novo pra validação.
</contexto>

<escopo>
  <arquivos_permitidos>
    supabase/functions/fernando-vaz-start/index.ts
    supabase/functions/_shared/fernando-vaz/prompts.ts
  </arquivos_permitidos>
  <arquivos_proibidos>
    supabase/functions/fernando-vaz/index.ts (lógica ok)
    supabase/functions/whatsapp-webhook/index.ts (fix LID feito)
    supabase/functions/_shared/fernando-vaz/types.ts
    supabase/functions/_shared/fernando-vaz/kits-catalog.ts
    Qualquer outra edge function
    Schema/defaults da tabela brand_identity (Nocast depende)
    Tenant Nocast (de274ffb-abe0-41de-9f4e-7c9ac49a68a4)
    Tenant "Andre Test FV" (preservar pra análise)
    agent_semantic_memory, agent_procedural_memory
  </arquivos_proibidos>
</escopo>

<tarefa>

# PASSO 1 — Patch fernando-vaz-start/index.ts

Adicionar UPSERT em brand_identity logo após criar whatsapp_sessions,
ANTES de enviar greeting.

Motivo: sobrescrever defaults Nocast com NULLs explícitos. Garantir
que a row existe pra UPDATEs subsequentes do main handler não caírem
em 0 rows.

## Localizar

No arquivo, procurar onde ocorre o `.from('whatsapp_sessions').insert(...)`.
Logo após esse insert ter sucesso, antes do `sendWhatsApp(greeting)`.

## Inserir

```typescript
// Criar brand_identity neutra (sobrescreve defaults Nocast da tabela)
const { error: biError } = await supabaseClient
  .from('brand_identity')
  .upsert({
    tenant_id,
    onboarding_step: 0,
    color_primary: null,
    color_secondary: null,
    color_text: null,
    color_accent: null,
    color_primary_dark: null,
    font_headline: null,
    font_body: null,
    font_weight: null,
    logo_url: null,
    logo_initial: null,
    logo_text_a: null,
    logo_text_b: null,
    label_breaking_a: null,
    label_breaking_b: null,
    cta_see_more: null,
    cta_carousel: null,
    portal_tone: null,
    portal_niche: null,
    portal_differencial: null,
  }, { onConflict: 'tenant_id' });

if (biError) {
  console.error('[fernando-vaz-start] brand_identity upsert failed:', biError);
  // Não aborta — greeting ainda deve sair
}
```

## Deploy

```bash
supabase functions deploy fernando-vaz-start --no-verify-jwt
```

---

# PASSO 2 — Patch prompts.ts

Adicionar uma nova seção ao system prompt documentando os nomes
reais das colunas. Colocar ANTES do bloco de "Steps" (ou da explicação
de updates.brand_identity/updates.tenants, onde existir).

## Inserir no system prompt

```markdown
## CAMPOS REAIS DA TABELA (use EXATAMENTE esses nomes em updates)

NUNCA invente nomes. Use apenas os listados abaixo.

### updates.brand_identity

Identidade editorial:
- portal_niche         ex: "jornalismo regional RN"
- portal_tone          ex: "técnico e acessível, sem buzzword"
- portal_differencial  ex: "cobertura hiperlocal 24/7"

Cores (formato hex #RRGGBB):
- color_primary        cor principal
- color_secondary      cor secundária
- color_accent         cor de destaque
- color_text           cor de texto
- color_primary_dark   versão escura de primary

Tipografia:
- font_headline        ex: "Anton, sans-serif"
- font_body            ex: "Inter, sans-serif"
- font_weight          ex: "700"

Logo e textos:
- logo_url             URL após Human-as-Tool upload
- logo_initial         letra principal, ex: "N"
- logo_text_a          primeira metade do nome, ex: "NEO"
- logo_text_b          segunda metade, ex: "TECH"

Labels e CTAs:
- label_breaking_a     ex: "URGENTE"
- label_breaking_b     ex: "AGORA"
- cta_see_more         ex: "SAIBA MAIS"
- cta_carousel         ex: "ARRASTE"

Estilo livre (jsonb) — para qualquer coisa que NÃO cabe nos campos acima:
- style_profile        jsonb, estrutura livre
- design_system        jsonb, estrutura livre
- co_creation_answers  jsonb, respostas do processo
- elements             jsonb, ex: {glow: true, particles: false}

### updates.tenants

- name, niche, whatsapp_number, kit_slug, tone
- onboarding_completed (boolean, true quando todos campos obrigatórios coletados)

### REGRAS CRÍTICAS

1. NUNCA inventar nome de campo. Se o usuário mencionar algo que não
   encaixa, usar style_profile (jsonb) em brand_identity.

2. portal_niche é do NEGÓCIO do usuário (o que ele faz).
   tenants.niche é sinônimo — preencher AMBOS com o mesmo valor.

3. portal_tone é o tom de comunicação. Quando o usuário descrever
   personalidade da marca (técnico, formal, casual), mapear pra portal_tone.

4. Cores: converter descrições em hex. "azul escuro" → #1E3A8A, "amarelo
   dourado" → #F5A800. Se ambíguo, escolher valor razoável — preferível a
   deixar NULL.

5. Depois de coletar 3-4 campos de uma "seção" (editorial, visual, logo),
   emitir action=advance_step + new_step correspondente. NÃO esperar
   completar TODOS os campos da tabela — step avança por seção lógica.
```

## Deploy

```bash
supabase functions deploy fernando-vaz --no-verify-jwt
```

(redeploy do fernando-vaz. O prompt é lido em runtime pelo main handler.)

---

# PASSO 3 — VALIDAÇÃO

## 3.1 — Criar tenant de teste pós-fix (novo, não reusar)

```sql
INSERT INTO tenants (name, slug, active, status, whatsapp_number, kit_slug, onboarding_completed)
VALUES (
  'Andre Test FV Pós-Fix',
  'test-fv-posfix-' || extract(epoch from now())::bigint::text,
  true, 'trial', '5584996967963', null, false
)
RETURNING id;
```

Copiar UUID → `TEST_TENANT_ID_2`.

## 3.2 — Disparar fernando-vaz-start

```bash
curl -X POST "https://erfeiyxfrutreckzpkeb.supabase.co/functions/v1/fernando-vaz-start" \
  -H "Content-Type: application/json" \
  -H "x-agent-secret: socialagent2026" \
  -d '{
    "tenant_id": "<TEST_TENANT_ID_2>",
    "phone": "5584996967963",
    "evolution_instance": "neotech-evolution"
  }'
```

## 3.3 — Verificar brand_identity criada com NULLs (CRÍTICO)

```sql
SELECT 
  tenant_id, 
  onboarding_step,
  color_primary, 
  color_secondary, 
  font_headline, 
  logo_initial, 
  logo_text_a, 
  logo_text_b, 
  portal_tone, 
  portal_niche, 
  portal_differencial
FROM brand_identity 
WHERE tenant_id='<TEST_TENANT_ID_2>';
```

**Esperado ANTES do usuário conversar:**
- tenant_id preenchido
- onboarding_step = 0
- TODOS os outros campos = NULL

Se ALGUM campo aparecer com valor Nocast (preto/dourado/Anton/NO/CAST),
patch não pegou. Investigar antes de prosseguir.

## 3.4 — André conversa via WhatsApp

Mesma conversa da Neotech (dono de empresa de IA). Esperar receber greeting
no zap e responder naturalmente. Alvo: 6-8 turnos.

## 3.5 — Verificar persistência pós-conversa

```sql
SELECT 
  t.name, t.kit_slug, t.niche,
  b.onboarding_step,
  b.portal_niche, b.portal_tone, b.portal_differencial,
  b.color_primary, b.color_secondary,
  b.font_headline, b.font_body,
  b.logo_initial, b.logo_text_a, b.logo_text_b,
  (SELECT COUNT(*) FROM whatsapp_conversations WHERE tenant_id=t.id) as msgs,
  (SELECT COUNT(*) FROM agent_episodic_memory WHERE tenant_id=t.id) as episodes
FROM tenants t
LEFT JOIN brand_identity b ON b.tenant_id = t.id
WHERE t.id='<TEST_TENANT_ID_2>';
```

**Esperado:**
- portal_niche preenchido (ex: "empresa de IA / sistemas autônomos")
- portal_tone preenchido
- portal_differencial preenchido (dessa vez não pode ficar NULL)
- color_primary/secondary preenchidos (conforme usuário descreveu)
- font_headline/body preenchidos
- logo_text_a/b preenchidos (ex: NEO/TECH ou SOCIAL/AGENT)
- onboarding_step > 0

## 3.6 — Confirmar Nocast intacto

```sql
SELECT onboarding_step, portal_niche, color_primary, font_headline, logo_text_a
FROM brand_identity 
WHERE tenant_id='de274ffb-abe0-41de-9f4e-7c9ac49a68a4';
```

**Esperado:** dados Nocast intactos (onboarding_step=5, preto, Anton, "NO").

</tarefa>

<nao_fazer>
- NÃO alterar defaults da tabela brand_identity via ALTER COLUMN
- NÃO tocar no tenant Nocast
- NÃO deletar "Andre Test FV" (preservar pra comparação)
- NÃO alterar lógica de advance_step no código Fernando Vaz
- NÃO criar whitelist rígido de campos (documentação no prompt basta)
- NÃO tocar no fix LID do whatsapp-webhook
- NÃO mexer em fernando-vaz/index.ts (main handler está ok)
- NÃO alterar kits-catalog.ts nem types.ts
- NÃO mexer em agent_semantic_memory / agent_procedural_memory
</nao_fazer>

<verificacao>
Pré-patch:
```bash
# Confirmar versão atual
supabase functions list | grep fernando-vaz
```

Pós-patch (após deploy):
```sql
-- 1. Fernando Vaz ainda ativo
SELECT status FROM department_agents WHERE slug='fernando-vaz';
-- Esperado: active

-- 2. Camada zero intacta
SELECT COUNT(*) FROM agent_procedural_memory WHERE agent_name IS NULL;
-- Esperado: 14

SELECT COUNT(*) FROM agent_semantic_memory WHERE tenant_id IS NULL;
-- Esperado: 20

-- 3. Brand_identity do novo tenant com NULLs (passo 3.3 acima)

-- 4. Nocast intacto (passo 3.6 acima)
```
</verificacao>

<criterio_sucesso>
1. ✓ fernando-vaz-start cria row em brand_identity automaticamente
2. ✓ Row nova NÃO herda defaults Nocast (campos visuais NULL)
3. ✓ Após conversa, portal_niche, portal_tone, portal_differencial preenchidos
4. ✓ Cores e fontes gravam quando mencionadas pelo usuário
5. ✓ logo_initial, logo_text_a, logo_text_b preenchidos com valores da marca
6. ✓ onboarding_step avança durante conversa (LLM segue instrução do prompt)
7. ✓ Nocast intacto (tenant_id=de274ffb-... sem alterações)
8. ✓ "Andre Test FV" original preservado pra comparação

Se 6 dos 8 passaram: fix satisfatório, seguir pra próximo .md.
Se 8 dos 8: tag STABLE-FERNANDO-VAZ-PERSISTENCE-FIX-2026-04-23.
</criterio_sucesso>

<rollback>
```bash
git checkout HEAD~1 -- supabase/functions/fernando-vaz-start/index.ts
git checkout HEAD~1 -- supabase/functions/_shared/fernando-vaz/prompts.ts
supabase functions deploy fernando-vaz-start --no-verify-jwt
supabase functions deploy fernando-vaz --no-verify-jwt
```

Depois, limpar tenant de teste se não for deixar:
```sql
DELETE FROM agent_working_memory WHERE tenant_id='<TEST_TENANT_ID_2>';
DELETE FROM agent_conversations WHERE tenant_id='<TEST_TENANT_ID_2>';
DELETE FROM agent_episodic_memory WHERE tenant_id='<TEST_TENANT_ID_2>';
DELETE FROM agent_plans WHERE tenant_id='<TEST_TENANT_ID_2>';
DELETE FROM whatsapp_conversations WHERE tenant_id='<TEST_TENANT_ID_2>';
DELETE FROM whatsapp_sessions WHERE tenant_id='<TEST_TENANT_ID_2>';
DELETE FROM agent_logs WHERE tenant_id='<TEST_TENANT_ID_2>';
DELETE FROM brand_identity WHERE tenant_id='<TEST_TENANT_ID_2>';
DELETE FROM tenants WHERE id='<TEST_TENANT_ID_2>';
```
</rollback>

<apos_concluir>
Se o fix passou em todos os 8 critérios:

1. Tag git: `STABLE-FERNANDO-VAZ-PERSISTENCE-FIX-2026-04-23`

2. Reportar ao André:
   - TEST_TENANT_ID_2 preservado pra análise
   - Campos que ficaram preenchidos (evidência do fix)
   - Campos que ainda ficaram NULL (gap do LLM pra iterar depois)

3. Próximos .md na fila (não executar agora):
   - CLEANUP-TENANTS-TESTE.md (delete Andre Test FV + Pós-Fix)
   - ADD-OWNER-PHONE-COLUMN.md (separar whatsapp_number de owner_phone)
   - CRIAR-NEOTECH-TENANT-REAL.md (primeiro tenant legítimo)
   - CRIAR-SOCIAL-AGENT-TENANT.md

Se o fix falhou em critérios essenciais (1, 2, 3):
- Ler agent_logs do teste pra ver onde persistência quebrou
- Ajustar prompt ou lógica de upsert
- Não seguir pra criação de tenants reais até fix comprovado
</apos_concluir>
