---
name: Startup — ler ao iniciar toda sessão
description: Estado completo do SocialAgent. Infra, pipeline, canais, prioridades. Fonte: ORION-CONTEXT-COMPLETO.md (12/abr/2026).
type: project
originSessionId: 6d4288fe-5c00-4978-9862-3b7b0b2c9450
---
## Estado Atual (2026-04-12, sessão 2)

### Hardware — Lenovo G50-80 (temporário, placa-mãe do PC quebrou)
- Intel i7-5500U (4 threads), 8GB RAM
- Intel HD 5500 (sem GPU NVIDIA)
- SSD 120GB Kingston (OS, /) — 48GB livre
- SSD 240GB PNY (/mnt/projects) — 144GB livre
- Monitor ultrawide 2560x1080 via HDMI (primary)
- Ollama DESABILITADO — sem GPU

### Infraestrutura

| Serviço | Detalhe |
|---------|---------|
| Supabase | `erfeiyxfrutreckzpkeb` (us-east-2) — backend principal, Pro plan |
| Tenant Nocast | `de274ffb-abe0-41de-9f4e-7c9ac49a68a4` |
| Frontend | Vercel `nocast-social.vercel.app` (deploy principal, auto-deploy via GitHub) |
| Render Worker | `render-worker-324868875786.us-central1.run.app/render` (GCP) |
| ML Quality Check | `ml-quality-check-324868875786.us-central1.run.app/predict` (GCP, EfficientNet-B0) |
| RSS Bridge | `rss-bridge-324868875786.us-central1.run.app` (GCP) |
| GCP Project | `design-ai-socialagent` (us-central1) |
| Repo | `andrelucasa7x/social-agent` (privado) |
| Código local | `/mnt/projects/docs/orion/NEO-TECH/nocast-social/` |
| WhatsApp | Evolution API — `558491671407@s.whatsapp.net` (Claudionor) |

### Stack LLM (ordem de prioridade)
1. **Gemini 2.5 Flash** — primário, gratuito 1000/dia
2. **DeepSeek** — raciocínio + fallback
3. **Claude** — diagnóstico via ti-agents
4. **OpenAI** — último fallback + DALL-E

### Canais de publicação (TODOS ATIVOS)
- Instagram — @nocast.oficial (account_id `17841453400906955`, System User token)
- Facebook — Page token configurado
- WhatsApp — Evolution API broadcast
- WordPress — nocast.com.br (token admin)

### Pipeline principal (NUNCA TOCAR)
```
news-curator → score-suggestions → agent-editor-chefe → generate-content
→ auto-improve-loop → render-card → quality-check → publish-post → process-schedule
```

### Copiloto WhatsApp (OPERACIONAL)
- copiloto-scheduler v3 — ciclo horário 6h-22h BRT
- copiloto-urgente v3 — alertas viral >= 9 a cada 2min
- whatsapp-webhook v6 — 5 handlers
- diario-oficial-notifier v2 — pede foto DOM

### Números do banco (12/abr/2026, verificado via SQL)
- ~46 edge functions ativas (12 deletadas na limpeza)
- ~18 cron jobs ativos (3 cancelados na limpeza)
- 489 fontes RSS (tabela sources)
- 64 tabelas no schema public
- 11.534 content_suggestions
- 984 posts (~170 com card_image_url, 15 publicados)
- 18.237 agent_logs
- 1.363 agent_memory (com embeddings vetoriais)
- 154 agent_insights
- 69 design_feedback
- 1 channel_analytics (collect-ig-insights v5 populando — 6765 followers, 0.33% engagement)

### Growth Loop — STATUS (atualizado 12/abr/2026, sessão 2)
Publicar ✅ → Coletar métricas IG ✅ → taste-learner ✅ → **quality-check ✅ (v18, scores 8.9-9.8)** → Ajustar conteúdo ⏳ (aguarda dados 7-14 dias) → Publicar melhor ⏳
O ciclo está **completo com quality data real**. quality-check retorna scores do ML (EfficientNet-B0). Dados acumulando.

### Nocast em números reais (12/abr/2026)
- 6.765 seguidores | 4.028 seguindo | 1.011 posts
- Engagement rate: 0.33% (baixo — meta saudável: 1-3%)
- Copiloto + broadcast + taste-learner vão elevar isso organicamente

### Tags estáveis
- `v1.0-stable` (commit `3e874de`) — MVP fechado
- `STABLE-COPILOTO-2026-04-12` — Copiloto deployed
- `STABLE-GROWTH-LOOP-2026-04-12` — Growth loop completo

### O que foi feito HOJE (12/abr/2026, sessões 2-3)

#### TAREFA 1 — fix-auth-users deletada ✅
- Senha hardcoded "123456" removida do Supabase

#### TAREFA 2 — Limpeza de 12 functions + 3 crons ✅
- 12 edge functions deletadas: design-researcher, design-evolver, storymaker, rd-department, rd-template-generator, frontend-rd, analyze-reference, design-agent, design-agent-v5, design-skill, smart-image-search, test-gemini
- 3 crons cancelados: rd-template-generator-12h, design-evolver-12h, storymaker-ig-scraper
- Redução: ~260 chamadas/24h desnecessárias eliminadas

#### TAREFA 3 — Fix imagem auto-improve-loop ✅
- auto-improve-loop v28 deployed
- Fix: pipeline_status CHECK constraint (`'needs_review'` → `'reviewing'`)
- image_url agora busca de content_suggestions primeiro (94% já têm)
- 5/5 cards renderizados e persistidos com card_image_url
- ~170 posts com cards, 814 no backlog (pipeline-manager-30min processando)

#### quality-check v18 — Fix crash ✅
- Root cause: `mem.plan()` chamado no top-level antes de `mem` ser definido → ReferenceError
- Fix: removidas 2 linhas de dead code, corrigido agent_logs columns
- ML endpoint GCP vivo, retornando scores 8.9-9.8
- Growth loop agora tem quality data real

#### Dashboard redesign ✅
- dashboard/page.tsx reescrito: 7 blocos consumindo dashboard-cockpit API
- Blocos: Header, Instagram Analytics, KPIs, Pipeline Funnel, Semana, Oportunidades, Performance
- Deployed no Vercel (commit c90e547)

#### auto-improve-loop v11.2 (sessão 3) ✅
- **Pexels removido** — nunca mais stock photos
- **Cruzamento de fontes RSS** (`searchSimilarImage`) — 489 fontes cruzam matérias, prioriza fonte com imagem acessível
- **WhatsApp threshold = 8.5** — só notícia muito quente sem imagem pede foto ao Claudionor
- **Notícia fria sem imagem** → skip silencioso (`skipped_no_image`)
- **Broadcast WhatsApp** — notícia 8.5+ publicada no IG → link do post IG enviado pra todos os contatos do Claudionor pra aumentar alcance
- Version 31 deployed, ACTIVE

#### agent-editor-chefe v16 — Carousel eligibility gate ✅
- `isCarouselEligible()` — gate por categoria + keywords antes do score
- Categorias diretas: economia, saude, tecnologia, ciencia
- Keywords tech/IA: inteligência artificial, chatgpt, blockchain, nvidia, startup, etc.
- Keywords tendências: biohacking, longevidade, comportamento digital, etc.
- Esportes, rn, cidade, politica, mundo genérico → SEMPRE card
- 20 registros errados no DB limpos (reset carousel_recommended = false)

#### Frontend fixes ✅
- Notícias: filtro score 5 → 7 (commit ff3f2ee)
- Carrosseis: query `queue_type=carousel` → `carousel_recommended=true` (commit 61641d5)
- Deploy manual Vercel (auto-deploy quebrado)

#### Vercel auto-deploy restaurado ✅
- Causa: GitHub App integration desconfigurada (zero webhooks)
- Fix: `vercel git disconnect` + `vercel git connect` — reconectou
- Push no main agora trigga deploy automático novamente
- Não precisa mais de `npx vercel --prod --yes`

### Pendências restantes (em ordem)
1. MÉDIA: Fix `analytics-agent` (bug coluna seo_meta)
2. MÉDIA: Migrar `agent-analista` + `agent-estrategista` pra Gemini
3. BAIXA: agent_logs do auto-improve-loop não persistem (column mismatch pré-existente)
4. FEATURE: Carrossel score > 8 → WhatsApp pro Claudionor aprovar e agendar publicação. Agent escolhe melhor horário.
5. FEATURE: Métricas de Notícias e Carrosseis — tornar úteis e acionáveis.

### REGRA CRÍTICA
Código pode ser modificado MAS sempre manter marcos seguros (git tag). Se quebrar: rollback pro tag estável. NUNCA estaca zero.

### REGRA: NUNCA RODAR LOCAL
O projeto NÃO roda local. Push pro GitHub → Vercel auto-deploya. Edge functions via Supabase MCP/CLI. Nunca `npm run dev`, nunca `localhost`.
