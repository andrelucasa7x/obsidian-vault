# SocialAgent — Master Plan

## Estado atual (24/Mar/2026)

### O que funciona
- 25 templates Manus parametrizados (data-slot + CSS vars)
- Renderer DOM injeta conteúdo nos slots
- 6 agents que agem (photo-finder, visual-qa, taste-update, ml-learn, scraper, template-evolver)
- Supabase V2 limpo (12 tabelas)
- 80 portais, 108 referências, 582 feedbacks

### O que NÃO funciona
1. GPT inventa hierarquia na headline
2. ML não aprende de verdade com feedbacks
3. Validação cara (GPT Vision)
4. Publicação manual
5. Sem aprendizado visual estruturado

## Solução

### Modelos locais (com [[RTX 3060]])
- **Texto:** Ollama + Llama 3 8B (grátis, local)
- **Visão:** Florence-2 (análise de referências + validação)
- **Layout Model:** Random Forest (recomenda template + parâmetros)
- **Headline:** Slots predefinidos (contexto → keyword → complemento)

### Pipeline novo
```
Referências (prints/links) → Florence-2 → Perfil de Design
Notícia → Layout Model → Template + Parâmetros
GPT → Preenche slots da headline
Renderer → Card 1080x1080
Florence-2 → Valida → Se OK, entrega
Feedback → Layout Model retreina
```

### Entregas técnicas
- `lib/ollama.ts` — wrapper Ollama
- `lib/florence.ts` — wrapper Florence-2
- `/training` page — interface de feedback
- Script Python — treino Layout Model

## Visão Final
Agência de marketing IA autônoma. Aprende visualmente. Publica automaticamente. Evolui com feedback. Escala pra qualquer nicho.

## Relacionado
[[Visao-Produto]] | [[Problemas-V1]] | [[Machine-Learning]] | [[DNA-Visual-NOCAST]] | [[OR1ON]] | [[Claudionor]]
