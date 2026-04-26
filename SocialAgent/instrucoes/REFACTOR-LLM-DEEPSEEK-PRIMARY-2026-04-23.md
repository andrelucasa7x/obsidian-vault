# REFACTOR FERNANDO VAZ LLM CHAIN — DEEPSEEK PRIMARY

<contexto>
Dados produção 24h (23 requests):
  DeepSeek    12s avg  ✓ estável
  Kimi k2.5   49s avg  ✗ 4x mais lento
  Kimi k2.6   84s avg  ✗ inviável

Kimi perde no Fernando Vaz. Inverter ordem: DeepSeek primary,
Kimi k2.5 fallback 1, Moonshot v1 fallback 2.
Temperature volta pra 0.7 em DeepSeek. Mantém temp=1 só no Kimi.
Baseline: v17 ACTIVE, commit STABLE-FERNANDO-VAZ-CLEAN-2026-04-23.
</contexto>

<escopo>
  <arquivos_permitidos>
    supabase/functions/fernando-vaz/index.ts
  </arquivos_permitidos>
  <arquivos_proibidos>
    whatsapp-webhook, fernando-vaz-start, _shared/*,
    Nocast tenant, Neotech tenant, outros edge functions,
    bloco Vision GPT-4o inteiro (VISION_EXTRACTION_PROMPT e helpers)
  </arquivos_proibidos>
</escopo>

<tarefa>
Em callLLM():
1. Inverter ordem: try DeepSeek → catch Kimi k2.5 → catch Moonshot
2. DeepSeek primary: deepseek-chat, temp 0.7, instrumentação
3. Kimi fallback 1: kimi-k2.5, temp 1 fixo, mesmo código
4. Moonshot fallback 2: inalterado
5. Preservar logs llm_fallback nos catch de DeepSeek e Kimi
Deploy v17 → v18.
</tarefa>

<nao_fazer>
- NÃO mexer bloco Vision
- NÃO mexer signature callLLM()
- NÃO remover instrumentação
- NÃO trocar modelo deepseek-chat por outro
</nao_fazer>

<verificacao>
grep -n "api.deepseek.com\|api.moonshot.ai\|kimi-k2\|deepseek-chat" supabase/functions/fernando-vaz/index.ts
→ DeepSeek URL antes de Kimi
</verificacao>

<criterio_sucesso>
1. v18 ACTIVE, zero warnings
2. 1 msg texto
3. provider='deepseek', latency < 20s
4. Zero llm_fallback
</criterio_sucesso>

<rollback>
git checkout STABLE-FERNANDO-VAZ-CLEAN-2026-04-23 -- supabase/functions/fernando-vaz/index.ts
supabase functions deploy fernando-vaz --no-verify-jwt
</rollback>

<apos_concluir>
Tag STABLE-FERNANDO-VAZ-DEEPSEEK-PRIMARY-2026-04-23
Próximo: CREATE-WEBDESIGNER-AGENT.md
</apos_concluir>
