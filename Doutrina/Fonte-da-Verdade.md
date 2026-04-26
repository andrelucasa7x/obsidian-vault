# Hierarquia de Fonte da Verdade

> Decisao de 21/abr/2026 — impacto de 2 anos

---

## Regra geral
Quando duas fontes divergem, a fonte da verdade ganha. A outra e derivada e deve ser atualizada.

## Hierarquia

| Informacao | Fonte da Verdade | Derivados |
|------------|-----------------|-----------|
| **Skill executavel** (conteudo, prompt, steps) | `~/.claude/skills/**/*.md` | Registry do Router (skills_classified.json) |
| **Agent executavel** (prompt, tools) | `~/.claude/agents/*.md` | Registry do Router (agents_classified.json) |
| **Registry indexado** (embeddings, classificacao) | `orion-router/registry/` | Derivado de skills/ e agents/ via build_index.py |
| **Quando usar skill/agent** (contexto, evolucao) | Vault Obsidian | — |
| **Decisoes de projeto** | Vault Obsidian | — |
| **Codigo do sistema** | Repo git (GitHub) | Deploy (Vercel, GCP) |
| **Estado da producao** (dados, metricas) | Supabase (banco) | Vault (snapshot manual) |
| **Configuracao** | `.env` + `config.py` | Vault (documentacao) |
| **Identidade visual** | Templates Manus (HTML) | Vault (DNA-Visual.md = doc, nao fonte) |

## Fluxo de sincronizacao

```
Skill criada/editada em ~/.claude/skills/
    │
    ├── scan_inventory.py atualiza registry
    ├── auto_classify.py enriquece metadata
    ├── build_index.py gera embeddings
    │
    └── Vault Obsidian: atualiza nota de contexto (manual)
        └── Quando usar, historico, decisoes
```

## Regras praticas

1. **Nunca editar skill no Vault** — edite em `~/.claude/skills/`, documente no Vault
2. **Numeros do banco sao snapshot** — Estado-Atual.md e foto de uma data, nao live
3. **Templates Manus sao a fonte** — DNA-Visual.md documenta mas nao substitui
4. **Config calibrado vive no codigo** — `config.py` e a verdade, Vault documenta
5. **Decisoes vivem no Vault** — nao em comentarios de codigo, nao em commits

## Quando atualizar o Vault

- **Decisao tomada** → nota imediata
- **Deploy feito** → atualizar Estado-Atual.md se mudou algo relevante
- **Skill nova** → nota de contexto (opcional, so se nao-obvio)
- **Metricas mudaram** → atualizar numeros no proximo uso

---
Ver tambem: [[Principios]] | [[Regras]]
