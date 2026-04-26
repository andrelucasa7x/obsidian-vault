# UPDATE FERNANDO VAZ PROMPT — KNOWLEDGE BASE + CREATIVE MODE

> Persona FV + 19 skills (~17.5K tokens). Dual LLM: DeepSeek conversa, Claude Sonnet 4.6 HTML.

<contexto>
FV hoje só onboarder. Enriquecer com 19 skills do projeto Claude.ai.
Arquitetura: 1 agent, 2 modos, roteamento por regex no handler.
Bundle: /tmp/fv-skills-full.md (61KB, 1525 linhas).
ANTHROPIC key validada via health-check: claude-sonnet-4-6 200 OK.
Baseline: v18 DeepSeek primary.
</contexto>

<escopo>
  <arquivos_permitidos>
    supabase/functions/_shared/fernando-vaz/skills-knowledge.md (NOVO)
    supabase/functions/_shared/fernando-vaz/prompts-creative.ts (NOVO)
    supabase/functions/fernando-vaz/index.ts (editar)
  </arquivos_permitidos>
  <arquivos_proibidos>
    whatsapp-webhook, fernando-vaz-start, kits-catalog.ts
    Vision GPT-4o block
    callLLM() DeepSeek/Kimi/Moonshot chain
    Qualquer outra edge function
    Tenants
  </arquivos_proibidos>
</escopo>

<tarefa>
1. cp /tmp/fv-skills-full.md → _shared/fernando-vaz/skills-knowledge.md
2. Criar _shared/fernando-vaz/prompts-creative.ts (CREATIVE_PROMPT + isCreativeRequest)
3. Editar fernando-vaz/index.ts: import + branch creativo pós-destructure + função callClaudeCreative
4. Deploy v18 → v19
</tarefa>

<nao_fazer>
- NÃO mexer Vision block
- NÃO mexer callLLM()
- NÃO integrar render-card (fase 2)
- NÃO enviar HTML pro zap (só log)
- NÃO tocar webhook/start/kits-catalog
- NÃO criar tenant teste
- NÃO criar WebDesigner separado
</nao_fazer>

<verificacao>
ls _shared/fernando-vaz/skills-knowledge.md → ~61K
grep isCreativeRequest|CREATIVE_PROMPT prompts-creative.ts → matches
grep callClaudeCreative|creative_generated|claude-sonnet fernando-vaz/index.ts → matches
</verificacao>

<criterio_sucesso>
1. Bundle em shared ~61K
2. prompts-creative.ts com CREATIVE_PROMPT + isCreativeRequest
3. index.ts com callClaudeCreative + branch creativo
4. v19 ACTIVE zero warnings
5. "gera um card teste" → agent_logs creative_generated com HTML válido
6. html_preview começa com <!DOCTYPE html>
7. "oi" continua DeepSeek ~4s
8. Nocast + pipeline Manus intocados
</criterio_sucesso>

<rollback>
git checkout STABLE-FERNANDO-VAZ-DEEPSEEK-PRIMARY-2026-04-23 -- supabase/functions/fernando-vaz/index.ts supabase/functions/_shared/fernando-vaz/
supabase functions deploy fernando-vaz --no-verify-jwt
</rollback>

<apos_concluir>
Tag: STABLE-FV-CREATIVE-MODE-2026-04-23
Próximo: PIPELINE-RENDER-CARD-FV-CREATIVE.md
</apos_concluir>
