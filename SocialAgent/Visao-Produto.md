# SocialAgent — Visão do Produto

**O que é:** Agência de marketing IA completa. "Photoshop no Modo God."
**Stack:** Next.js 14 + React 18 + TypeScript + Tailwind 3.4 + Supabase + GPT-4o + Gemini Flash

## Pipeline de Geração
1. Notícia chega (RSS ou manual)
2. Photo Finder busca foto real
3. GPT escolhe melhor dos 25 templates
4. GPT adapta headline (60 chars, hierarquia 3 níveis)
5. Renderer injeta dados no HTML via data-slot
6. HCTI renderiza HTML → PNG 1080x1080
7. Visual QA valida
8. Cliente aprova → publica IG/WP/WhatsApp

## 25 Templates Manus
HERO_FULL | SPLIT_SIDE | CINEMA_TOP | BOLD_TYPE | FRAME_BOX | SPLIT_DATA | TECH_METRICS | MAGAZINE | TECH_LAUNCH | GLASS_CARD | MINIMAL_LINE | QUOTE_CARD | DATA_CARD | HERO_SPLIT_V | COUNTDOWN | POLL_CARD | TIMELINE | BREAKING_ALERT | SPORT_SCORE | PROFILE_CARD | LIST_CARD | EXPLAINER | WEATHER_CARD | OPINION_CARD | STORY_CARD

## 6 Agents V2
| Agent | O que faz |
|-------|-----------|
| photo-finder | Busca melhor foto via SearchAPI/og:image |
| visual-qa | GPT Vision valida card |
| taste-update | Processa feedback → atualiza preferências |
| ml-learn | GPT Vision analisa portais → regras |
| scraper | Apify coleta posts de 80 portais |
| template-evolver | Gemini cria variações dos aprovados |

## Métricas Ultron (meta)
- 90%+ aprovação nos cards
- 0 erros de renderização
- Publicação automática WP+IG+WhatsApp
- 100+ clientes, 1000+ posts/dia
- Custo < R$0.05/post
- 5+ clientes pagando, R$1.500-3.500 MRR

## Relacionado
[[DNA-Visual-NOCAST]] | [[Claudionor]] | [[OR1ON]] | [[ClipAI]]
