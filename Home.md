# NEOTECH — Central de Comando

> Atualizado: 21/abr/2026

---

## Produto Principal
- [[SocialAgent/Estado-Atual|SocialAgent — Estado Atual]] — infra, pipeline, canais, numeros
- [[SocialAgent/Visao-Produto|SocialAgent — Visao do Produto]] — SaaS, roadmap, agencia autonoma
- [[SocialAgent/Master-Plan|SocialAgent — Master Plan]] — trajetoria copiloto → autonomo

## Arquitetura
- [[Arquitetura/Infraestrutura|Infraestrutura Completa]] — Supabase + Vercel + GCP + Hardware
- [[Arquitetura/Orion-Router|Orion Router v1.1]] — MCP Router, Gemini, fallback chains
- [[Arquitetura/Pipeline|Pipeline de Producao]] — RSS → score → editor → render → publish
- [[Arquitetura/Stack-LLM|Stack de LLMs]] — Gemini, DeepSeek, Claude, OpenAI

## Projetos

```dataview
TABLE status AS "Status"
FROM "Projetos"
SORT status ASC
```

## Doutrina
- [[Doutrina/Principios|13 Principios NEOTECH]]
- [[Doutrina/Camadas|11 Camadas do Sistema]]
- [[Doutrina/Regras|Regras de Execucao]]
- [[Doutrina/Fonte-da-Verdade|Hierarquia de Fonte da Verdade]]

## Identidade
- [[Identidade/DNA-Visual|DNA Visual NOCAST]] — cores, fontes, templates
- [[Identidade/Templates|10 Templates Manus]] — sagrados, nunca alterar

## Pessoas
- [[Pessoas/Andre|Andre Lucas]] — fundador
- [[Pessoas/Claudionor|Claudionor]] — 1o cliente, jornalista

## Holding
- [[NEO-HOLDING/Estrutura-Corporativa|Estrutura Corporativa]] — NEO HOLDING > NEOTECH

## Ferramentas
- [[Ferramentas/Stack-Completo|Stack Completo]] — APIs, servicos, custos
- [[Ferramentas/Arsenal-AI|Arsenal AI]] — 425 skills + 174 agents
- [[Referências/BibliotecaDev|BibliotecaDev]] — 70+ livros

## Marketing
- [[Marketing/Gatilhos-Mentais/CURIOSIDADE|Gatilhos Mentais]]
- [[Marketing/Emails/EMAIL---Sequência-de-Boas-Vindas|Sequencias de Email]]
- [[Marketing/Headlines/|Headlines]]

## Notas Recentes

```dataview
TABLE file.mtime AS "Modificado"
FROM ""
WHERE file.name != "Home"
SORT file.mtime DESC
LIMIT 10
```

## Diario

```dataview
LIST
FROM "Daily"
SORT file.name DESC
LIMIT 7
```
