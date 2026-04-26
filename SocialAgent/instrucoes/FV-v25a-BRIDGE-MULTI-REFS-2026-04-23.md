# FERNANDO VAZ v25a — BRIDGE + MULTI-REFERÊNCIAS

<contexto>
v24 cego pro brief e recebe ref única (gold_standard) → Claude copia.
Fix: fetch brief (agent_plans + agent_conversations) + selecionar 3 refs via keywords.
Vision QA e image generation ficam pra v25b.
</contexto>

<escopo>
  <arquivos_permitidos>
    supabase/functions/fernando-vaz/index.ts
    supabase/functions/_shared/fernando-vaz/prompts-creative.ts (ajuste REFERÊNCIA)
  </arquivos_permitidos>
  <arquivos_proibidos>
    skills-knowledge.md, prompts.ts, pipeline render, Vision, webhook, schema
  </arquivos_proibidos>
</escopo>

<tarefa>
1. Adicionar fetchBriefFromSession(supabase, tenant_id, session_id) antes de callClaudeCreative
2. Adicionar selectReferences(brief_summary, user_message) retornando até 3 URLs
3. Modificar callClaudeCreative: receber session_id, chamar fetchBrief + selectRefs, userContent multimodal com brief + N refs + instrução final
4. Retornar brief + reference_urls no resultado pra logging
5. Call-site: passar session_id
6. Log creative_rendered: +brief_plan_name, brief_goals_count, brief_recent_msgs_count, reference_count, reference_urls
7. Prompt: substituir REFERÊNCIA VISUAL (singular) por REFERÊNCIAS VISUAIS ANEXADAS (plural, 1-3 imagens) + regra "BRIEF tem PRIORIDADE sobre imagens"
8. Deploy v24 → v25
</tarefa>

<nao_fazer>
- NÃO implementar Vision QA
- NÃO implementar geração imagem externa
- NÃO retry Claude
- NÃO mexer skills/prompts/render/webhook
- NÃO testar via curl
- NÃO remover v24 replacements
</nao_fazer>

<verificacao>
grep "fetchBriefFromSession|selectReferences|BRIEF DA CONVERSA|reference_urls|REF_MAP" → 8+ matches
</verificacao>

<criterio_sucesso>
1. v25 ACTIVE zero warnings
2. Teste "manifesto IA": card não copia imobiliária
3. refs = array de 3 URLs diferentes
4. brief_goals_count >= 1 OR brief_recent_msgs_count >= 3
5. Vermelho + Parnamirim RN preservados de v24
6. Latência +5-10s vs v24
</criterio_sucesso>

<rollback>
git checkout STABLE-FV-CREATIVE-HARDENED-v2-2026-04-23 -- supabase/functions/fernando-vaz/index.ts supabase/functions/_shared/fernando-vaz/prompts-creative.ts
supabase functions deploy fernando-vaz --no-verify-jwt
</rollback>

<apos_concluir>
Tag STABLE-FV-CREATIVE-BRIDGE-MULTI-REFS-2026-04-23.
Próximo v25b: Vision QA passivo + geração Nano Banana/DALL-E.
</apos_concluir>
