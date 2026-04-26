# REFINE FV CREATIVE — ANTI-COPY + VERMELHO OBRIGATÓRIO + LOGO GRANDE + LOCATION

<contexto>
v23 falhou: Claude copiou composição gold_standard, vermelho ausente (0 matches),
logo 28px, footer "São Paulo, SP" inventado.
Coluna location criada + populada via MCP (Parnamirim, RN pros 2 tenants).
</contexto>

<escopo>
  <arquivos_permitidos>
    supabase/functions/_shared/fernando-vaz/prompts-creative.ts
    supabase/functions/fernando-vaz/index.ts (SELECT + replacements)
  </arquivos_permitidos>
  <arquivos_proibidos>
    skills-knowledge.md, prompts.ts, pipeline render, Vision, webhook
    Schema brand_identity (MCP já fez)
  </arquivos_proibidos>
</escopo>

<tarefa>
1. Adicionar 'location' no SELECT brand_identity
2. Adicionar {{location}} no replacements (fallback 'Parnamirim, RN')
3. Prompt: adicionar linha location ao bloco BRAND_IDENTITY
4. Prompt: substituir REFERÊNCIA VISUAL com regras anti-copy + proibições composição
5. Prompt: adicionar REGRAS DE IDENTIDADE OBRIGATÓRIAS (vermelho obrigatório, logo 48px+, footer location literal, escolha 1 dos 6 tipos)
6. Prompt: adicionar CHECKPOINT FINAL pre-output
7. Deploy v23 → v24
</tarefa>

<nao_fazer>
- NÃO mexer skills-knowledge, prompts.ts
- NÃO alterar schema via código
- NÃO mudar modelo Claude
- NÃO remover multimodal
- NÃO testar via curl
</nao_fazer>

<verificacao>
grep "PROIBIÇÕES ABSOLUTAS DE COMPOSIÇÃO|VERMELHO (color_secondary) obrigatório|LOGO dimensões mínimas|CHECKPOINT FINAL|{{location}}" → 7+ matches
</verificacao>

<criterio_sucesso>
1. v24 ACTIVE
2. HTML tem #d92d20 pelo menos 1 match
3. logo-img height >= 48px
4. Footer "Parnamirim, RN"
5. Composição ≠ gold_standard
6. 1 dos 6 tipos canônicos
</criterio_sucesso>

<rollback>
git checkout STABLE-FV-CREATIVE-MANUAL-v1-2026-04-23 -- supabase/functions/_shared/fernando-vaz/prompts-creative.ts supabase/functions/fernando-vaz/index.ts
supabase functions deploy fernando-vaz --no-verify-jwt
</rollback>

<apos_concluir>
Tag STABLE-FV-CREATIVE-HARDENED-v2-2026-04-23.
Se falhar: remover imagem multimodal OU temperature 0.7.
</apos_concluir>
