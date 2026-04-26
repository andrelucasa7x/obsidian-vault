# Regras de Execucao

> Regras duras. Nao negociaveis.

---

## Codigo
- **Plan Mode e o default** — nunca acceptEdits direto
- **So executar .md com escopo XML rigido** — contexto, escopo, tarefa, nao_fazer, verificacao, criterio_sucesso, rollback
- **Nunca extrapolar escopo** — se diz "modifique X", so modifica X
- **Nunca "melhorar" codigo** sem ser pedido
- **Verificacao obrigatoria no fim** — rodar todos os comandos de verificacao

## Pipeline
- **NUNCA tocar no pipeline** — ele funciona
- **NUNCA rodar local** — push → Vercel auto-deploy
- **Sempre manter tags git** — rollback seguro
- **Templates Manus sao sagrados** — renderer so preenche slots

## Identidade Visual
- Logo NUNCA como PNG — sempre HTML/CSS
- Foto NUNCA ultrapassa limites do template
- Badge so 1 por card
- Accent dourado #F5A800 e lei absoluta

## Orion Router
- Usar 2-4 semanas em producao antes de mexer
- `analyze_usage.py` so apos 100+ chamadas reais
- Confidence 0.46 e cosmetico — nao "resolver"

## Social Agent
- Codigo e **marco** — nao alterar sem instrucao
- Core fica local/privado — nunca GitHub publico
- NUNCA criar codigo novo quando ja existe
- NUNCA perguntar "quer que eu faca?" — AGIR

---
Ver tambem: [[Principios]] | [[Camadas]]
