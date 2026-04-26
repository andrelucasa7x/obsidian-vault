# Stack de LLMs

> Atualizado: 21/abr/2026

---

## Hierarquia (ordem de prioridade)

### 1. Gemini 2.5 Flash (PRIMARIO)
- **Uso**: Classificador do Orion Router + Vision (Video2Knowledge)
- **Modelo**: `gemini-2.5-flash-lite`
- **Custo**: Gratuito (free tier, 1000 req/dia)
- **Latencia**: ~1.4s
- **Auth**: Google Cloud ADC (neoholdingtech@gmail.com)

### 2. DeepSeek-V3
- **Uso**: Fallback classificador + enriquecimento do registry + raciocinio
- **Modelo**: `deepseek-chat`
- **Custo**: ~$0.001/query
- **Latencia**: ~5s

### 3. Claude
- **Uso**: Diagnostico via ti-agents, Claude Code (executor local)
- **Modelos**: Opus 4.6, Sonnet 4.6, Haiku 4.5
- **Custo**: Plano Claude Code

### 4. OpenAI
- **Uso**: Embeddings (registry) + ultimo fallback + DALL-E
- **Modelo embeddings**: `text-embedding-3-small` (1536 dims)
- **Custo**: ~$0.00001/query (embeddings)

## Principio NEOTECH #8: Fallback Chains

```
Classificador:  Gemini → DeepSeek → None
Vision (V2K):   Gemini → GPT-4o → None
Embeddings:     OpenAI (unico, sem fallback)
Edge functions: Gemini → DeepSeek → Claude → OpenAI
```

Cada engine falha silenciosamente e passa pro proximo. Nunca erro total.

## APIs e Custos

| API | Uso | Custo |
|-----|-----|-------|
| Gemini 2.5 Flash Lite | Classify + Vision | Gratis |
| DeepSeek-V3 | Classify fallback + enrichment | ~$0.001/q |
| OpenAI emb-3-small | Embeddings | ~$0.00001/q |
| GPT-4o Vision | V2K fallback | ~$0.01/frame |
| yt-dlp + ffmpeg + Whisper | Video extraction | Gratis (local) |

## Google Cloud (projeto design-ai-socialagent)

| Servico | Status |
|---------|--------|
| Gemini API | ATIVO — classificador + vision |
| Vertex AI | Disponivel (nao usado) |
| Cloud Vision | Disponivel |
| Speech-to-Text | Disponivel |
| Translation | Disponivel |
| Natural Language | Disponivel |
| Text-to-Speech | Disponivel |
| Drive API v3 | Inoperante (OAuth scope) |

---
Ver tambem: [[Orion-Router]] | [[Infraestrutura]]
