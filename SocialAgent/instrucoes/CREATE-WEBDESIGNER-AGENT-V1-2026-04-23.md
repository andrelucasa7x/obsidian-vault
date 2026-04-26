# CREATE WEBDESIGNER AGENT V1 — PARTE 1 de 3

> Edge function webdesigner-agent + storage bucket. MVP standalone.

---

<contexto>
FV onboarder puro, delega design pra agents especializados. Kit 'agencia'
tem ZERO templates ativos hoje. Neotech precisa disso. Postador (Nocast, 10 Manus)
permanece INTOCADO.

Render: GCP Cloud Run https://render-worker-324868875786.us-central1.run.app/render
Storage: bucket 'generated-assets' público, 10MB max, image/*
Modelo: Claude Sonnet 4.6 primary
Reference DNA: reference_styles id='39aefef5-a6a9-42f9-ab63-8d3328ca9c1f'
Baseline: STABLE-FERNANDO-VAZ-DEEPSEEK-PRIMARY-2026-04-23
</contexto>

<escopo>
  <arquivos_permitidos>
    supabase/functions/webdesigner-agent/index.ts (NOVO)
    supabase/functions/_shared/webdesigner/prompts.ts (NOVO)
    migrations SQL via MCP (bucket — já feito externo)
  </arquivos_permitidos>
  <arquivos_proibidos>
    fernando-vaz/index.ts (integração fica pro .md 2)
    whatsapp-webhook/index.ts
    fernando-vaz-start/index.ts
    _shared/fernando-vaz/*
    templates do kit postador
    tenant Nocast
    outras edge functions
  </arquivos_proibidos>
</escopo>

<tarefa>
PASSO 1: Bucket já criado via MCP (pular).
PASSO 2: Criar _shared/webdesigner/prompts.ts (WEBDESIGNER_SYSTEM_PROMPT + buildWebDesignerUserPrompt).
PASSO 3: Criar webdesigner-agent/index.ts (Claude Sonnet 4.6 → HTML → render-worker → Storage upload → URL).
PASSO 4: Deploy supabase functions deploy webdesigner-agent --no-verify-jwt.
</tarefa>

<nao_fazer>
- NÃO integrar com Fernando Vaz ainda
- NÃO criar templates kit agencia
- NÃO modificar fernando-vaz / webhook / publish-post
- NÃO tocar Nocast ou 10 templates sagrados
- NÃO usar Gemini/Kimi (só Claude Sonnet 4.6)
- NÃO implementar fallback LLM (MVP)
- NÃO cachear HTML
- NÃO rodar curl automatizado (André testa manual)
</nao_fazer>

<verificacao>
supabase functions list → webdesigner-agent ACTIVE
grep -n "ANTHROPIC_API_KEY|claude-sonnet-4-6|render-worker|generated-assets" → 4+ matches
</verificacao>

<criterio_sucesso>
1. Bucket generated-assets pronto
2. Edge function ACTIVE zero warnings
3. Curl teste retorna success:true + URL .png
4. URL renderiza no browser com DNA Neotech
5. agent_logs action='generate_asset' success
6. Latência < 45s
7. Nocast + FV intocados
</criterio_sucesso>

<rollback>
supabase functions delete webdesigner-agent
DELETE FROM storage.buckets WHERE id = 'generated-assets';
</rollback>

<apos_concluir>
Tag STABLE-WEBDESIGNER-V1-2026-04-23
Próximo: INTEGRATE-FERNANDO-VAZ-WEBDESIGNER.md
</apos_concluir>
