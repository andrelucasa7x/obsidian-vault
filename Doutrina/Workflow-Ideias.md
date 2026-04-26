# Workflow: Da Ideia ao Codigo

> Como uma ideia nasce, passa pelos departamentos e vira execucao

---

## Visao Geral

```
┌─────────────────────────────────────────────────────┐
│                   CLAUDE.AI                          │
│                                                      │
│  ┌──────────┐                                        │
│  │ IDEIAS   │  ← Tudo comeca aqui                    │
│  │ (inbox)  │  "Quero X" / "E se Y?" / "Preciso Z"  │
│  └────┬─────┘                                        │
│       │ classifica e roteia                           │
│       ▼                                              │
│  ┌──────────────────────────────────────────┐        │
│  │         DEPARTAMENTOS                     │        │
│  │                                           │        │
│  │  Criacao ──→ Conteudo ──→ Marketing       │        │
│  │     │            │            │            │        │
│  │     ▼            ▼            ▼            │        │
│  │  Engenharia ← T.I. ← Inteligencia        │        │
│  │     │                                     │        │
│  │     ▼                                     │        │
│  │  Distribuicao                              │        │
│  │     │                                     │        │
│  │  Fernando Vaz (consulta em qualquer ponto)│        │
│  └──────────────┬───────────────────────────┘        │
│                 │                                     │
│  ┌──────────────▼──────────────┐                     │
│  │ SOCIAL AGENT (cockpit)      │                     │
│  │ Consolida tudo              │                     │
│  │ Gera .md de execucao        │                     │
│  └──────────────┬──────────────┘                     │
│                 │                                     │
└─────────────────┼─────────────────────────────────────┘
                  │
                  │  .md com XML rigido
                  ▼
┌─────────────────────────────────────────────────────┐
│              CLAUDE CODE (executor)                  │
│                                                      │
│  Recebe .md → valida escopo → executa → verifica     │
│  Orion Router encontra skills/agents certos           │
│  Resultado volta pro Obsidian + Claude.ai             │
└─────────────────────────────────────────────────────┘
```

---

## PROJETO 0: IDEIAS (inbox)

**Este e o projeto que falta criar no Claude.ai.**

Tudo comeca aqui. Ideia crua, sem forma. Pode ser:
- "Quero que o NOCAST publique carrosseis sozinho"
- "Preciso monetizar o portal"
- "E se a IA respondesse DMs?"
- "O engagement ta baixo, preciso melhorar"

### Instructions do projeto IDEIAS

```
Voce e o Diretor de Inovacao da NEOTECH.

Seu papel: receber ideias cruas do Andre e transformar em briefings estruturados.

Para CADA ideia, voce deve:

1. ENTENDER — O que o Andre quer? Qual o problema real?
2. CLASSIFICAR — Qual departamento lidera?
   - Visual/template → Criacao
   - Texto/headlines/copy → Conteudo
   - Ads/growth/funil → Marketing
   - ML/dados/metricas → Inteligencia
   - Canais/publicacao → Distribuicao
   - Infra/deploy/cloud → T.I.
   - Codigo/frontend/backend → Engenharia
   - Pesquisa/experimento → P&D
   - Copy/oferta/criativo → Fernando Vaz
3. ROTEAR — Dizer ao Andre: "Abre o projeto [DEPARTAMENTO] e cola isso:"
4. GERAR BRIEFING — Um bloco de contexto pro departamento entender a demanda

Formato do briefing de roteamento:

---
IDEIA: [nome curto]
DEPARTAMENTO LIDER: [qual]
DEPARTAMENTOS DE APOIO: [quais consultar]
CONTEXTO: [o que precisa saber]
OBJETIVO: [o que entregar]
RESTRICOES: [o que nao fazer]
PROXIMO PASSO: abrir projeto [X] e colar este briefing
---

Regras:
- NUNCA pular pra execucao — ideias se pensam aqui, se executam no Claude Code
- Se a ideia envolve mais de 1 departamento, definir SEQUENCIA
- Se a ideia precisa de pesquisa antes, rotear pra P&D primeiro
- Se envolve copy/oferta, SEMPRE consultar Fernando Vaz
```

---

## Protocolo de Handoff entre Departamentos

Quando um departamento termina sua parte, gera um **handoff** pro proximo:

```
---
HANDOFF DE: [departamento origem]
HANDOFF PARA: [departamento destino]
IDEIA: [nome]
O QUE FOI FEITO: [resumo do que este departamento decidiu]
O QUE FALTA: [o que o proximo departamento precisa resolver]
ARQUIVOS: [se gerou algo, onde esta]
---
```

### Exemplo: "Quero carrosseis automaticos"

```
1. IDEIAS classifica → Criacao (lider) + Conteudo + Engenharia

2. Andre abre CRIACAO:
   "Preciso de template de carrossel NOCAST. 5 slides."
   Criacao define: layout, cores, slots, specs
   → Handoff pra CONTEUDO

3. Andre abre CONTEUDO:
   "Template de carrossel pronto. Preciso definir:
   como o editor-chefe decide card vs carrossel?"
   Conteudo define: regras de roteamento, copy por slide
   → Handoff pra ENGENHARIA

4. Andre abre ENGENHARIA:
   "Template + regras prontos. Preciso da edge function
   carousel-ai que gera os slides."
   Engenharia define: arquitetura, API, testes
   → Gera .md de execucao

5. Andre abre SOCIAL AGENT (cockpit):
   Consolida tudo. Valida. Gera .md final com XML rigido.

6. Andre cola .md no CLAUDE CODE:
   Eu executo.
```

---

## Sequencias Comuns

### Feature nova (ex: carrossel, monetizacao)
```
IDEIAS → P&D (pesquisa) → departamento lider → apoios → ENGENHARIA → SOCIAL AGENT → CLAUDE CODE
```

### Bug ou fix (ex: engagement baixo)
```
IDEIAS → INTELIGENCIA (diagnostico) → departamento responsavel → SOCIAL AGENT → CLAUDE CODE
```

### Campanha (ex: lancamento, promo)
```
IDEIAS → Fernando Vaz (estrategia) → MARKETING (plano) → CRIACAO (pecas) → CONTEUDO (copy) → DISTRIBUICAO → SOCIAL AGENT → CLAUDE CODE
```

### Melhoria visual (ex: novo template)
```
IDEIAS → CRIACAO → CONTEUDO (copy adapta) → ENGENHARIA (implementa) → SOCIAL AGENT → CLAUDE CODE
```

### Infra (ex: migrar API, novo servico)
```
IDEIAS → T.I. (avalia) → ENGENHARIA (implementa) → SOCIAL AGENT → CLAUDE CODE
```

---

## Regras do Workflow

1. **Tudo comeca em IDEIAS** — nunca pular direto pra departamento
2. **SOCIAL AGENT e o ultimo antes de executar** — consolida e valida
3. **Fernando Vaz e consultor** — pode ser chamado em qualquer ponto
4. **Handoffs sao obrigatorios** — nenhum departamento assume sem contexto
5. **Claude Code so recebe .md pronto** — nunca ideia crua
6. **Resultado volta pro Obsidian** — documenta o que aprendeu

---

## Mapa Final dos Projetos Claude.ai

```
IDEIAS (inbox — tudo comeca aqui)
    │
    ├─→ Fernando Vaz (consultor — chamado de qualquer lugar)
    │
    ├─→ P&D (pesquisa antes de construir)
    │
    ├─→ Criacao (visual, templates)
    ├─→ Conteudo (copy, headlines)
    ├─→ Marketing (ads, growth, funis)
    ├─→ Inteligencia (ML, dados, metricas)
    ├─→ Distribuicao (canais, publicacao)
    ├─→ T.I. (infra, deploy)
    ├─→ Engenharia (codigo, testes)
    │
    └─→ SOCIAL AGENT (cockpit — consolida e gera .md)
              │
              ▼
         CLAUDE CODE (executa)
              │
              ▼
         OBSIDIAN (documenta)
```
