# REFINE FV CREATIVE — MANUAL.md v1.0 + VISUAL REFERENCES + VISION MULTIMODAL

<contexto>
Fase 3 branch creativo. v22 sai parecido Nocast. Fix: usar todos campos
brand_identity + Vision multimodal (gold standard como image content).
brand_identity Neotech já corrigida via MCP (logo_url real, Unbounded,
cores manual). 35 arquivos subidos em generated-assets/neotech-brand/.
</contexto>

<escopo>
  <arquivos_permitidos>
    supabase/functions/_shared/fernando-vaz/prompts-creative.ts
    supabase/functions/fernando-vaz/index.ts (só callClaudeCreative)
  </arquivos_permitidos>
  <arquivos_proibidos>
    skills-knowledge.md, prompts.ts, Vision GPT-4o block
    pipeline render, webhook
    Tenants (só leitura)
  </arquivos_proibidos>
</escopo>

<tarefa>
1. Expandir SELECT brand_identity pra incluir instagram_handle, website_url, reference_images
2. Substituir FERNANDO_VAZ_CREATIVE_PROMPT com DOUTRINA MANUAL.md v1.0 (anatomia 5 posições, 6 tipos, tom→composição, niche→elemento)
3. Substituir section brand com bloco BRAND_IDENTITY usando placeholders {{campo}}
4. callClaudeCreative: replaceAll({{...}}) + user content multimodal (image gold_standard + text)
5. Deploy v22 → v23
</tarefa>

<nao_fazer>
- NÃO mexer skills, prompts.ts, Vision, render, webhook
- NÃO remover fallback se brand faltar
- NÃO mais de 1 imagem no messages
- NÃO testar via curl
- NÃO mudar max_tokens
</nao_fazer>

<verificacao>
grep "replaceAll|goldStandardUrl|type: 'image'|{{color_primary}}|portal_tone" → 8+ matches
</verificacao>

<criterio_sucesso>
1. v23 ACTIVE zero warnings
2. Card Unbounded headline, cores #000+#d92d20+#34c759
3. Logo neotech oficial
4. Footer @neotech.app + site + logo
5. Zero Nocast/dourado/Barlow/N letter
6. HTML inicia <!DOCTYPE html>
</criterio_sucesso>

<rollback>
git checkout STABLE-FV-CREATIVE-HARDENED-2026-04-23 -- supabase/functions/_shared/fernando-vaz/prompts-creative.ts supabase/functions/fernando-vaz/index.ts
supabase functions deploy fernando-vaz --no-verify-jwt
</rollback>

<apos_concluir>
Tag STABLE-FV-CREATIVE-MANUAL-v1-2026-04-23.
Se ainda genérico: próximo .md few-shot description.
Se URL image falhar no Anthropic fetch: converter gold_standard pra base64.
</apos_concluir>
