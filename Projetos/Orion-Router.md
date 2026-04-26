---
status: ATIVO
---
# Orion Router

> Status: **ATIVO — em producao** | v1.1

---

## O que e
MCP Server que roteia tarefas do Claude Code para os 425 skills + 174 agents mais relevantes. Economia de ~99.3% de tokens por query.

## Stack
- Python (2.881 loc)
- Gemini 2.5 Flash Lite (classificador primario)
- DeepSeek-V3 (fallback)
- OpenAI emb-3-small (embeddings)
- SQLite (cache)

## Metricas
- Agent hit: 85%
- Skill hit: 90%
- Category: 100%
- MCP match: 100%

## Inclui Video2Knowledge
Pipeline YouTube → knowledge .md (Gemini vision + Whisper local).

## Localizacao
- `/home/andre/orion-router/` (privado, sem GitHub)
- Symlink: `/mnt/projects/projetos/orion-router/`

## Proximo passo
Usar 2-4 semanas em producao. `analyze_usage.py` apos 100+ chamadas reais.

---
Ver tambem: [[../Arquitetura/Orion-Router|Arquitetura Detalhada]] | [[../Arquitetura/Stack-LLM|Stack LLM]]
