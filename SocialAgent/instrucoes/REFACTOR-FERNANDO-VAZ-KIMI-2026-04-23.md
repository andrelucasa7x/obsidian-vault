# REFACTOR FERNANDO VAZ LLM — GEMINI → KIMI K2

> Substitui primary Gemini 2.5 Flash por Kimi K2 (agentic-capable, 200K context).
> Fallback DeepSeek V3.2 mantido. Instrumenta logs com llm_provider +
> model + latency pra auditoria futura.
> 1 arquivo, 1 bloco editado, 1 deploy, 1 pré-requisito (secret Kimi).

---

<contexto>
Fernando Vaz hoje (v11 pós-cleanup) usa callLLM() local em
supabase/functions/fernando-vaz/index.ts linha ~437.
- Bloco Gemini atual: linhas 438-475 (chama generativelanguage.googleapis.com)
- Bloco fallback DeepSeek: já existe depois do Gemini
- Bloco Vision (GPT-4o): intocado, não faz parte desse .md

Health check pós-deploy ephemeral confirmou (23/04 06:40):
  KIMI       NO_KEY    ← bloqueante, resolver antes
  DEEPSEEK   200       ✓ fallback funciona
  GEMINI     200       ✓ continua como backup-de-backup se quiser
  OPENAI     200       ✓ (Vision GPT-4o independente)
  ANTHROPIC  200       ✓ (reservado pra WebDesigner futuro)

Por que Kimi K2 pro Fernando Vaz:
- Moonshot AI kimi-k2-0905-preview é agentic-capable → bom pra
  Human-as-Tool future (invocar WebDesigner, MediaStrategist, etc)
- 200K context → cabe histórico longo de conversa sem truncar
- Endpoint OpenAI-compatible → migração trivial
- Custo competitivo vs Gemini 2.5 Flash em conversa longa
- Liberta Gemini pro volume (news-curator, headline, quality-check)

Instrumentação nova: agent_logs hoje não registra qual LLM foi usado.
Esse .md adiciona metadata.llm_provider + metadata.model +
metadata.latency_ms em todo log de produção do Fernando Vaz.
Audit trail pra comparar performance entre providers depois.

Baseline: STABLE-FERNANDO-VAZ-CLEAN-2026-04-23 (commit 568f057,
fernando-vaz v11 ACTIVE, 510 linhas).

Endpoint Kimi K2 (confirmado via health-check /v1/models):
  URL:     https://api.moonshot.ai/v1/chat/completions
  Model:   kimi-k2.6  (262K context, supports reasoning + vision)
  Auth:    Authorization: Bearer <KIMI_API_KEY>
  Format:  OpenAI Chat Completions compatible

Fallback chain:
  1. kimi-k2.6                    (primary)
  2. deepseek-chat                (fallback 1 — provider diverso)
  3. moonshot-v1-128k             (fallback 2 — mesma conta Moonshot, estável)
</contexto>

<escopo>
  <arquivos_permitidos>
    supabase/functions/fernando-vaz/index.ts
  </arquivos_permitidos>
  <arquivos_proibidos>
    whatsapp-webhook/index.ts
    fernando-vaz-start/index.ts
    _shared/*
    Todo schema do banco
    Qualquer tenant
    Todas as outras edge functions
    Bloco Vision (extractBrandFromImage, VISION_EXTRACTION_PROMPT,
      mapVisionToBrandIdentity, buildVisionReply)
  </arquivos_proibidos>
</escopo>

<tarefa>
PASSO 0: Validar KIMI_API_KEY em secrets via health-check efêmero (HTTP 200).
PASSO 1.1: Localizar callLLM() e bloco Gemini (linhas ~438-475).
PASSO 1.2: Substituir bloco Gemini por bloco Kimi (api.moonshot.ai, model kimi-k2-0905-preview).
PASSO 1.3: Preservar fallback DeepSeek, adicionar campos de instrumentação no return.
PASSO 1.4: Instrumentar agent_logs com llm_provider/llm_model/llm_latency_ms/llm_tokens_*.
PASSO 1.5: Deploy fernando-vaz (v11 → v12).
</tarefa>

<nao_fazer>
- NÃO tocar bloco Vision
- NÃO alterar signature de callLLM()
- NÃO remover fallback DeepSeek
- NÃO criar _shared/llm-client.ts
- NÃO tocar webhook ou fernando-vaz-start
- NÃO alterar prompts
- NÃO trocar model sem justificativa
- NÃO commit se KIMI_API_KEY ausente
</nao_fazer>

<verificacao>
Pré-deploy: grep api.moonshot.ai + kimi-k2 deve retornar matches; grep deepseek preservado; grep gemini-2 ideal zero ou só comentário.
Pós-deploy: agent_logs metadata.llm_provider='kimi', llm_latency_ms numérico.
</verificacao>

<criterio_sucesso>
1. KIMI_API_KEY validada 200
2. v12 ACTIVE zero warnings
3. Grep Kimi URL + model presentes
4. DeepSeek preservado
5. Gemini primary removido (ou doc como 3º fallback)
6. Msg texto teste responde coerente
7. agent_logs llm_provider='kimi' preenchido
8. latency_ms numérico razoável (1000-5000ms)
9. onboarding_step preservado
10. Nocast intacto
</criterio_sucesso>

<rollback>
git checkout STABLE-FERNANDO-VAZ-CLEAN-2026-04-23 -- supabase/functions/fernando-vaz/index.ts
supabase functions deploy fernando-vaz --no-verify-jwt
</rollback>

<apos_concluir>
Tag: STABLE-FERNANDO-VAZ-KIMI-2026-04-23
Próximo backlog: CREATE-WEBDESIGNER-AGENT (Sonnet 4.6), REFACTOR-AUTO-IMPROVE-LOOP, MULTIMODAL-AUDIO-WHISPER.
Observar 24-48h latência/qualidade Kimi antes de expandir padrão.
</apos_concluir>
