---
name: DNA Visual NOCAST V2 — Padrão dos cards aprovados
description: Padrão visual extraído dos cards v4 aprovados pelo Andre. LEI ABSOLUTA pra geração de cards.
type: project
---

# DNA Visual NOCAST V2 — Cards Aprovados

Extraído de: prf-v4b.png, test-prf-gogeta.png, breaking-v4.png (26/mar/2026)

## Logo
- **Posição**: canto superior esquerdo, padding ~40px
- **Formato**: ícone dourado circular (play) + "NOCAST" em branco bold
- **Background**: pill semi-transparente escura atrás
- **Tamanho**: ~36px altura do ícone, texto proporcional

## Badge Categoria
- **Posição**: canto superior direito, padding ~40px
- **Cor**: vermelho sólido (#E53935) ou dourado (#F5A800) dependendo da categoria
- **Texto**: branco, uppercase, bold, letter-spacing
- **Shape**: retangular, border-radius ~4px

## Headline (3 NÍVEIS OBRIGATÓRIOS)
- **Nível 1** (contexto): branco, bold, ~40-48px
- **Nível 2** (KEYWORD): VERMELHO (#E53935) para segurança/urgência, DOURADO (#F5A800) para geral
  - **2x maior** que os outros níveis (~72-96px)
  - Itálico bold
- **Nível 3** (complemento): branco, bold, ~40-48px
- **Posição**: bottom-left, sobre o overlay

## Foto
- **Full bleed**: cobre o card inteiro como background
- **Overlay**: gradiente de baixo pra cima
  - Bottom: rgba(0,0,0,0.95)
  - Middle: rgba(0,0,0,0.5)
  - Top: rgba(0,0,0,0.1)
- **Nunca sem foto** — se não tem, usar gradiente de cor

## Tag Categoria (acima da headline)
- "● SEGURANÇA" — bolinha colorida + texto uppercase, letter-spacing largo
- Cor: vermelho pra segurança, dourado pra geral

## Descrição (sub)
- Abaixo da headline, 1-2 linhas
- Branco com opacidade ~0.7, font menor (~16px)
- Pode ter emoji no início

## Footer
- **Esquerda**: `@nocast.oficial · nocast.com.br` — branco, ~14px, opacity 0.5
- **Direita**: CTA botão — fundo dourado/vermelho, texto branco bold, uppercase
  - ">> VEJA O QUE ACONTECEU" / ">> LEIA MAIS" / ">> SAIBA MAIS"

## Borda
- Borda fina (~2px) colorida em volta do card inteiro
- Cor: vermelha pra segurança, dourada pra geral

## Cores
- Preto (#111, #1a1a1a) — fundo/overlay
- Branco (#fff) — texto principal
- Vermelho (#E53935) — keyword segurança, badges urgentes
- Dourado (#F5A800, #FFB300) — keyword geral, CTA, accent
- Variação: azul escuro pra política

## How to apply
Todos os templates devem seguir esse padrão. Qualquer card que não tenha 3 níveis de headline, foto full bleed, e footer com CTA = REJEITADO.
