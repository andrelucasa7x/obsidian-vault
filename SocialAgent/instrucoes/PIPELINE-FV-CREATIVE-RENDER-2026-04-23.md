# PIPELINE FV CREATIVE → RENDER → ZAP

> HTML → render-worker GCP → Storage → Evolution sendMedia.

<contexto>
v20: branch creative gera HTML 8KB (Claude Sonnet 4.6, 38s).
Gap: HTML não persistido; cliente não vê card.
Fase 2: render + upload + sendMedia. MVP.
Bucket 'generated-assets' aceita PNG+JPEG+WEBP+HTML.
Render-worker: https://render-worker-324868875786.us-central1.run.app/render
Evolution sendMedia: /message/sendMedia/{instance}
Baseline: v20 STABLE-FV-CREATIVE-MODE.
</contexto>

<escopo>
  <arquivos_permitidos>
    supabase/functions/fernando-vaz/index.ts (branch creative)
  </arquivos_permitidos>
  <arquivos_proibidos>
    prompts-creative.ts, skills-knowledge.md, prompts.ts
    callLLM() chain
    Vision block
    webhook, fernando-vaz-start
    Outras edge functions
    Schema, tenants (só leitura)
  </arquivos_proibidos>
</escopo>

<tarefa>
1. Constantes RENDER_WORKER_URL + STORAGE_BUCKET no topo
2. Expandir bloco if(htmlResult.success) com pipeline render/upload/sendMedia
3. Log creative_rendered OR creative_render_failed com png_url + html_url + latencies
4. Reply "Card gerado. Veja acima."
5. Deploy v20 → v21
</tarefa>

<nao_fazer>
- NÃO mexer prompts ou skills
- NÃO mexer modo conversa
- NÃO mexer Vision
- NÃO adicionar retry
- NÃO refinar qualidade HTML
- NÃO separar função render
- NÃO criar tabela nova
</nao_fazer>

<verificacao>
grep RENDER_WORKER_URL|sendMedia|creative_rendered|generated-assets → 5+ matches
</verificacao>

<criterio_sucesso>
1. v21 ACTIVE zero warnings
2. "gera um card" → imagem PNG no zap em ~60-90s
3. agent_logs creative_rendered com png_url + html_url
4. png_url abre imagem válida 1080x1080
5. DNA Neotech aplicado
6. Modo conversa OK
7. Nocast intacto
</criterio_sucesso>

<rollback>
git checkout STABLE-FV-CREATIVE-MODE-2026-04-23 -- supabase/functions/fernando-vaz/index.ts
supabase functions deploy fernando-vaz --no-verify-jwt
</rollback>

<apos_concluir>
Tag STABLE-FV-CREATIVE-RENDER-2026-04-23.
Próximo: REFINE ou novas features.
</apos_concluir>
