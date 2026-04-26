---
name: Carrossel — nichos e assuntos que viram carrossel
description: 4 nichos de carrossel (tech, economia, saúde, tendências). Estrutura 7 slides. Frontend+template já existem. Agent só precisa classificar.
type: project
originSessionId: 1db09bd4-ecbd-4637-b4c3-0b8c4949c336
---
## Carrossel — o que o agent precisa saber

O frontend, o template e a edge function `carousel-ai` **já existem**. O agent só precisa classificar quais sugestões vão pra aba de carrossel (não pra notícias).

### 4 nichos que viram carrossel

1. **"O Futuro Chegou" (Tech/IA)** — novas ferramentas, impacto no emprego, gadgets, lançamentos
2. **"E o Bolso?" (Economia/Finanças)** — mercado, geopolítica + preços, oportunidades
3. **"Biohacking" (Saúde/Ciência)** — estudos, longevidade, saúde mental, nutrição
4. **"Curadoria de Tendências" (Comportamento/Negócios)** — resumo da semana, análise de marca, cultura digital

### Estrutura dos 7 slides

| Slide | Conteúdo | Objetivo |
|-------|----------|----------|
| Capa | Gancho forte + imagem de impacto | Parar o scroll |
| Slide 2 | O problema ou notícia nua e crua | Contextualizar |
| Slide 3-5 | "Como isso te afeta" ou "Como usar" | Entregar valor real |
| Slide 6 | Opinião ou análise de especialista | Gerar autoridade |
| Slide 7 | CTA | Gerar comentário/compartilhamento |

### Diferencial: Newsjacking
Com 489 fontes RSS, o maior trunfo é pegar notícia que acabou de sair e ser o primeiro a explicar "o que fazer com essa informação" em formato visual.

### Regra de roteamento (ABSOLUTA)
- **Tech/IA, Economia/Finanças, Saúde/Ciência, Tendências/Negócios** → `carousel_recommended = true` → aba Carrosseis. **NÃO vai pra Notícias.**
- **Todas as outras categorias** → card single → aba Notícias.

### Fluxo do carrossel
1. Sugestão chega → agent classifica pela categoria → carousel ou card
2. Carousel: aparece na aba Carrosseis → clica "Gerar" → `carousel-ai` processa slides
3. Score > 8 → WhatsApp pro Claudionor **aprovar e agendar publicação**
4. Agent escolhe o **melhor horário** pra postar no IG como carousel

**Why:** Carrossel tem alcance orgânico maior no IG. Os 4 temas são o nicho do André — conteúdo denso que entrega utilidade ou impacto emocional.
