# Stack Completo

> Atualizado: 21/abr/2026

---

## LLMs e AI

| Servico | Uso | Custo |
|---------|-----|-------|
| Gemini 2.5 Flash Lite | Classificador + Vision | Gratis (free tier) |
| DeepSeek-V3 | Fallback + enrichment | ~$0.001/q |
| Claude (Opus/Sonnet/Haiku) | Executor local + diagnostico | Plano Claude Code |
| OpenAI emb-3-small | Embeddings | ~$0.00001/q |
| GPT-4o Vision | V2K fallback | ~$0.01/frame |
| fal.ai | Geracao de imagens/videos | Pay-per-use |

## Infraestrutura

| Servico | Uso |
|---------|-----|
| Supabase (Pro) | Backend: DB, Auth, Storage, Edge Functions, Crons |
| Vercel | Frontend: Next.js, auto-deploy |
| GCP Cloud Run | Workers: render, ML, RSS bridge |
| GitHub | Codigo: 8 repos privados |

## APIs e Servicos

| API | Funcao |
|-----|--------|
| Evolution API | WhatsApp automation |
| Pexels | Banco de imagens (REMOVIDO do pipeline) |
| HCTI | HTML → Image (legado) |
| Apify | Web scraping |
| RapidAPI | APIs diversas |

## Ferramentas Locais

| Ferramenta | Uso |
|------------|-----|
| Claude Code | Executor local (174 agents, 425 skills) |
| Obsidian | Knowledge base (este vault) |
| yt-dlp + ffmpeg | Download de video |
| Whisper (local) | Transcricao de audio |
| Ollama | LLM local (DESABILITADO — sem GPU) |

## Google Cloud (design-ai-socialagent)
- Auth: ADC (neoholdingtech@gmail.com)
- SA: vertex-brain@design-ai-socialagent.iam
- APIs: Gemini, Vertex AI, Vision, Speech-to-Text, Translation, NL API, TTS
- Drive API: inoperante (OAuth scope)

---
Ver tambem: [[Arsenal-AI]] | [[../Arquitetura/Stack-LLM|Stack LLM]]
