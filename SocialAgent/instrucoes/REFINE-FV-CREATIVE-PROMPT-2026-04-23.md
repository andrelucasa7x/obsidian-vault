# REFINE FV CREATIVE PROMPT — MULTI-TENANT BRAND ISOLATION

> FV copiando DNA Nocast em cards de outros tenants. Separar ESTRUTURA (skills)
> de IDENTIDADE (brand_identity). Hardening do system prompt.

<contexto>
Teste real: card Neotech saiu parecido com Nocast. brand_identity já
corrigida via MCP (logo_url placeholder removido, accent #00D87A verde real,
logo_text_a='neotech' logo_text_b='.').

Skills contêm 01-dna-visual-nocast.md + 06-templates-manus-referencia.md.
LLM lê como template visual → reproduz identidade Nocast em outros tenants.

Princípio arquitetural:
- Skills = ESTRUTURA (layouts, composições, princípios) reutilizável
- brand_identity = IDENTIDADE (cores, fonte, logo) única do tenant
- NUNCA misturar

Baseline: v21 STABLE-FV-CREATIVE-RENDER.
</contexto>

<escopo>
  <arquivos_permitidos>
    supabase/functions/_shared/fernando-vaz/prompts-creative.ts
  </arquivos_permitidos>
  <arquivos_proibidos>
    skills-knowledge.md (permanece intocável)
    fernando-vaz/index.ts
    prompts.ts
    Tenants
  </arquivos_proibidos>
</escopo>

<tarefa>
Substituir seção "## REGRAS ABSOLUTAS PARA GERAÇÃO DE HTML" em
FERNANDO_VAZ_CREATIVE_PROMPT por versão hardened:
- Aviso MULTI-TENANT separando estrutura/identidade
- IDENTIDADE obrigatoriamente de brand_identity injetado
- Construção explícita de logo com logo_initial/text_a/text_b
- Proibições quando tenant != Nocast (sem N, sem NOCAST, sem dourado, sem Barlow)
- Regra de PERGUNTAR se brand_identity faltar
- Output começa em <!DOCTYPE html>, ZERO antes

Deploy v21 → v22.
</tarefa>

<nao_fazer>
- NÃO alterar skills-knowledge.md
- NÃO remover arquivo Nocast das skills
- NÃO mexer isCreativeRequest / callClaudeCreative / pipeline render
- NÃO mexer modo conversa
- NÃO alterar brand_identity via código (MCP já fez)
</nao_fazer>

<verificacao>
grep "MULTI-TENANT|brand_identity injetado|logo_text_a|PROIBIÇÕES ABSOLUTAS" prompts-creative.ts → 4+ matches
</verificacao>

<criterio_sucesso>
1. v22 ACTIVE zero warnings
2. Card accent #00D87A (verde, não vermelho)
3. Logo "neotech." visível
4. Zero menção a Nocast
5. Estrutura similar OK, identidade Neotech real
</criterio_sucesso>

<rollback>
git checkout STABLE-FV-CREATIVE-RENDER-2026-04-23 -- supabase/functions/_shared/fernando-vaz/prompts-creative.ts
supabase functions deploy fernando-vaz --no-verify-jwt
</rollback>

<apos_concluir>
Tag STABLE-FV-CREATIVE-HARDENED-2026-04-23.
Se 2 cards seguidos ainda copiarem Nocast:
- Próximo .md: REMOVE-NOCAST-SKILLS-FROM-BUNDLE.md
</apos_concluir>
