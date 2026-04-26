# Orion Router v1.1

> Atualizado: 21/abr/2026 | 2.881 linhas Python | 425 skills + 174 agents

---

## O que e
MCP Server que roteia tarefas do Claude Code para os agents/skills mais relevantes. Economiza ~99.3% dos tokens (937k → 6.5k por query).

## Pipeline

```
    task do usuario
         │
         ▼
    ┌─────────────┐
    │ CACHE CHECK  │  SQLite (cosine >= 0.92)
    │ routes.db    │  HIT? → resposta imediata
    └──────┬──────┘
           │ MISS
           ▼
    ┌─────────────────────────────────┐
    │ CLASSIFY INTENT (fallback chain)│
    │                                 │
    │ Gemini 2.5 Flash Lite (~1.4s)  │
    │     ↓ fail                      │
    │ DeepSeek-V3 (~5s, $0.001)      │
    └──────┬──────────────────────────┘
           │
           ▼
    ┌─────────────────────────────────┐
    │ SEMANTIC SEARCH (cosine sim)    │
    │                                 │
    │ skills.npy  425 x 1536  TOP_K=5│
    │ agents.npy  174 x 1536  TOP_K=3│
    │ OpenAI emb-3-small             │
    └──────┬──────────────────────────┘
           │
           ▼
    ┌─────────────────────────────────┐
    │ BUNDLE RESPONSE + MCP suggest   │
    │ 10 MCPs: Gmail, Calendar, Drive │
    │ Canva, Notion, Playwright, etc  │
    └──────┬──────────────────────────┘
           │
           ▼
    ┌─────────────────────────────────┐
    │ CACHE + LOG                     │
    │ store_cache() + log_route()     │
    └─────────────────────────────────┘
```

## Metricas (20 test cases)
- Agent hit: **85%**
- Skill hit: **90%**
- Category: **100%**
- MCP match: **100%**

## Config Calibrado

| Parametro | Valor |
|-----------|-------|
| TOP_K_SKILLS | 5 |
| TOP_K_AGENTS | 3 |
| MIN_SCORE_SKILLS | 0.28 |
| MIN_SCORE_AGENTS | 0.25 |
| CACHE_SIMILARITY | 0.92 |
| GEMINI_MODEL | gemini-2.5-flash-lite |
| CLASSIFIER_PRIMARY | gemini |
| CLASSIFIER_FALLBACK | deepseek |

## Video2Knowledge
Pipeline que transforma videos YouTube em knowledge files (.md):
```
YouTube URL → yt-dlp + ffmpeg + Whisper (local)
→ Gemini Flash Lite (vision) [fallback: GPT-4o]
→ synthesizer → output/knowledge/*.md
```

## Mapa de Arquivos
```
~/orion-router/
├── server.py ········· MCP Server stdio (237 loc)
├── config.py ········· Thresholds + modelos (35 loc)
├── router/
│   ├── classifier.py · Gemini → DeepSeek fallback (110 loc)
│   ├── matcher.py ···· Cosine similarity (142 loc)
│   ├── bundler.py ···· Response + MCP suggest (117 loc)
│   └── cache.py ······ SQLite cache + logging (159 loc)
├── scripts/
│   ├── scan_inventory.py · Escaneia ~/.claude/ (375 loc)
│   ├── auto_classify.py ·· DeepSeek enrichment (332 loc)
│   ├── build_index.py ···· OpenAI embeddings (181 loc)
│   └── video2skill/ ······ YouTube → knowledge
├── registry/
│   ├── skills_classified.json (425)
│   ├── agents_classified.json (174)
│   ├── skills.npy (2.5 MB)
│   └── agents.npy (1.0 MB)
└── tests/
    └── test_router.py ···· 20 test cases
```

## Status
**Em producao.** Usar 2-4 semanas antes de qualquer mudanca. Proximo passo: `analyze_usage.py` apos 100+ chamadas reais.

## Debitos
- Drive API inoperante (baixa)
- Confidence 0.46 cosmetico (muito baixa)
- Sem observabilidade (media — proximo passo real)
- Rebuild incremental quando >5 skills/semana

---
Ver tambem: [[Infraestrutura]] | [[Stack-LLM]] | [[Pipeline]]
