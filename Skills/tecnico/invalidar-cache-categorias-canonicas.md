# Invalidar cache de categorias canônicas

## Descoberta
Cache em memória de categorias canônicas deve ser invalidado quando novas categorias são inseridas no banco, não apenas em cold start.

## Regra
Sempre que houver inserção de nova categoria na tabela categorias_canonicas, invalide o cache em memória (categoriasCache) para garantir que instâncias quentes reflitam a mudança. Não confie apenas em cold start para reconstruir o cache.

## Contexto
Ao implementar funcionalidades que envolvem cache de dados de referência que podem ser alterados em runtime, especialmente em sistemas com múltiplas instâncias.

## Origem
2025-04-10 - commit: fix(auto-improve-loop): v11-case-fix