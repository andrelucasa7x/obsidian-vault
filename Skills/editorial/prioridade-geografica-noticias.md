# Prioridade Geográfica para Notícias

## Descoberta
Notícias globais estavam sendo priorizadas indevidamente; a ordenação por prioridade geográfica e instrução explícita no prompt corrigem o problema.

## Regra
Ao selecionar notícias para manchete, priorize por ordem geográfica: Parnamirim > Câmara > Prefeitura > Estado > RN > Brasil > Mundo. Use ORDER BY geo_priority no SQL e inclua esta instrução no prompt do modelo.

## Contexto
Aplicar sempre que houver seleção de notícias para jornal diário, especialmente na definição de manchetes.

## Origem
2025-03-27, commit fix(jornal-diario): prioridade geográfica Parnamirim > RN > Brasil