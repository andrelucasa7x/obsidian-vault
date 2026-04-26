# SocialAgent — Estado Atual

> Atualizado: 21/abr/2026 | Fonte: producao real

---

## Hardware (temporario)
- **Lenovo G50-80** — i7-5500U, 8GB RAM, Intel HD 5500
- SSD 120GB Kingston (OS `/`) — ~50GB livre
- SSD 240GB PNY (`/mnt/projects`) — 152GB livre
- Monitor ultrawide 2560x1080
- Sem GPU — Ollama desabilitado
- Placa-mae do PC principal (Xeon + RTX 3060 12GB) quebrou

## Infraestrutura de Producao

| Servico | Detalhe |
|---------|---------|
| **Supabase** | `erfeiyxfrutreckzpkeb` (us-east-2), Pro plan |
| **Tenant Nocast** | `de274ffb-abe0-41de-9f4e-7c9ac49a68a4` |
| **Frontend** | Vercel — `nocast-social.vercel.app` (auto-deploy via GitHub) |
| **Render Worker** | GCP Cloud Run — `render-worker-324868875786.us-central1.run.app` |
| **ML Quality** | GCP Cloud Run — `ml-quality-check-324868875786.us-central1.run.app` (EfficientNet-B0) |
| **RSS Bridge** | GCP Cloud Run — `rss-bridge-324868875786.us-central1.run.app` |
| **GCP Project** | `design-ai-socialagent` (us-central1) |
| **Repo** | `andrelucasa7x/social-agent` (privado) |
| **WhatsApp** | Evolution API — `558491671407@s.whatsapp.net` |

## Stack LLM (prioridade)
1. **Gemini 2.5 Flash** — primario, gratuito 1000/dia
2. **DeepSeek** — raciocinio + fallback
3. **Claude** — diagnostico via ti-agents
4. **OpenAI** — ultimo fallback + DALL-E

## Canais de Publicacao (TODOS ATIVOS)
- **Instagram** — @nocast.oficial (System User token, sem expiracao)
- **Facebook** — Page token configurado
- **WhatsApp** — Evolution API broadcast
- **WordPress** — nocast.com.br (token admin)

## Pipeline Principal
```
news-curator → score-suggestions → agent-editor-chefe → generate-content
→ auto-improve-loop → render-card → quality-check → publish-post → process-schedule
```
**NUNCA TOCAR NO PIPELINE. ELE FUNCIONA.**

## Copiloto WhatsApp (OPERACIONAL)
- `copiloto-scheduler v3` — ciclo horario 6h-22h BRT
- `copiloto-urgente v3` — alertas viral >= 9 a cada 2min
- `whatsapp-webhook v6` — 5 handlers
- `diario-oficial-notifier v2` — pede foto DOM

## Numeros do Banco (12/abr/2026)
- ~46 edge functions ativas
- ~18 cron jobs ativos
- 489 fontes RSS
- 64 tabelas no schema public
- 11.534 content_suggestions
- 984 posts (~170 com card, 15 publicados)
- 18.237 agent_logs
- 1.363 agent_memory (com embeddings)

## Instagram @nocast.oficial
- 6.765 seguidores | 4.028 seguindo | 1.011 posts
- Engagement rate: 0.33% (meta: 1-3%)

## Growth Loop
```
Publicar → Coletar metricas IG → taste-learner → quality-check (ML) → Ajustar → Publicar melhor
```
Status: **completo com quality data real** (scores 8.9-9.8). Dados acumulando.

## Tags Estaveis (git)
- `v1.0-stable` (commit `3e874de`) — MVP fechado
- `STABLE-COPILOTO-2026-04-12` — Copiloto deployed
- `STABLE-GROWTH-LOOP-2026-04-12` — Growth loop completo

## Pendencias
1. MEDIA: Fix `analytics-agent` (bug coluna seo_meta)
2. MEDIA: Migrar `agent-analista` + `agent-estrategista` pra Gemini
3. BAIXA: agent_logs do auto-improve-loop (column mismatch)
4. FEATURE: Carrossel score > 8 → WhatsApp pro Claudionor aprovar
5. FEATURE: Metricas de Noticias e Carrosseis — tornar uteis

## Regras Criticas
- **NUNCA rodar local** — push → Vercel auto-deploy
- **NUNCA tocar no pipeline** — ele funciona
- **Sempre manter tags** — rollback seguro

---
Ver tambem: [[Visao-Produto]] | [[Master-Plan]] | [[../Arquitetura/Infraestrutura|Infraestrutura]]
