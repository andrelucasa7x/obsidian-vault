# FERNANDO VAZ v25 — BRIDGE + ÂNGULOS ROTATIVOS + MULTI-REFS

<contexto>
v25a (atual): bridge + multi-refs por keyword. Ainda falta rotação de ângulos.
v25 full: adiciona rotação (exclui últimos 3 angle_used), 10 ângulos canônicos,
Claude declara ângulo escolhido em comentário CSS, refs por categoria diversa.
Filosofia: ID VISUAL = fôrma fixa. ÂNGULO = recheio variável.
</contexto>

<escopo>
  <arquivos_permitidos>
    supabase/functions/fernando-vaz/index.ts
    supabase/functions/_shared/fernando-vaz/prompts-creative.ts
  </arquivos_permitidos>
  <arquivos_proibidos>
    skills-knowledge.md, prompts.ts (conversa)
    pipeline render (só adicionar metadata)
    Vision GPT-4o, webhook, schema
  </arquivos_proibidos>
</escopo>

<tarefa>
1. Substituir selectReferences por selectReferenceMirrors (diversidade por categoria via angleToCategory)
2. Adicionar selectAngleCandidates (lê últimos 3 angle_used do agent_logs, exclui, ranqueia keyword match dos 7 restantes, top3)
3. Adicionar ALL_ANGLES (10 ângulos canônicos)
4. callClaudeCreative: chamar selectAngleCandidates + selectReferenceMirrors, adicionar {{angle_candidates}} + {{angles_excluded_recent}} nos replacements, regex extract /* ÂNGULO: X */ + /* JUSTIFICATIVA: ... */ do HTML
5. Retornar angles_offered, angles_excluded_recent, angle_used, angle_justification
6. Log creative_rendered: +angle_used, +angle_justification, +angles_offered, +angles_excluded_recent
7. Prompt: inserir "REGRA DA FÔRMA DE BOLO" antes BRAND_IDENTITY, "ÂNGULO DO CARD" depois BRAND_IDENTITY, substituir seção REFERÊNCIAS VISUAIS com ESPELHOS DE IDENTIDADE + hierarquia de prioridade
8. Deploy v25 → v26
</tarefa>

<nao_fazer>
- NÃO implementar Vision QA / geração externa / retry (v26+)
- NÃO criar tabelas
- NÃO mexer skills/prompts/render/webhook
- NÃO testar via curl
- NÃO remover replacements v24/v25a
- NÃO mais de 3 imagens no userContent
- NÃO remover PROIBIÇÕES ABSOLUTAS DE COMPOSIÇÃO v24
</nao_fazer>

<verificacao>
grep "fetchBriefFromSession|selectAngleCandidates|selectReferenceMirrors|ALL_ANGLES|REGRA DA FÔRMA DE BOLO|ÂNGULO DO CARD|angle_used|angles_offered|{{angle_candidates}}" → 14+ matches
</verificacao>

<criterio_sucesso>
1. v26 ACTIVE zero warnings
2. TESTE 1 "manifesto IA": angle_used=TESE, sem mock/foto
3. TESTE 2 brief conversa: brief_goals_count > 0 OR msgs > 5
4. TESTE 3 seco: 3 refs de categorias diferentes
5. angle_used nunca null (Claude declarou)
6. angles_offered array 3 strings
7. #d92d20 + Parnamirim RN preservados
8. 3 cards seguidos: angle_used varia (rotação OK)
</criterio_sucesso>

<rollback>
git checkout STABLE-FV-CREATIVE-HARDENED-v2-2026-04-23 -- supabase/functions/fernando-vaz/index.ts supabase/functions/_shared/fernando-vaz/prompts-creative.ts
supabase functions deploy fernando-vaz --no-verify-jwt
</rollback>

<apos_concluir>
Tag STABLE-FV-CREATIVE-ANGLES-v25-2026-04-23.
Próximo v26: Vision QA + geração externa imagem.
Se Claude não declara ângulo: adicionar validação regex + retry.
Se rotação falhar: debug query agent_logs metadata.
</apos_concluir>
