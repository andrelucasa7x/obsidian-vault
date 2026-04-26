# Validar existência de template ao carregar canônicas

## Descoberta
Slugs de template inválidos causam falhas silenciosas em selectTemplate; é necessário validar existência ao carregar canônicas.

## Regra
Ao carregar categorias canônicas, valide que todos os slugs de template (template_default e templates_alt) existem no sistema. Caso contrário, registre erro e corrija ou remova a referência inválida. Nunca confie que um slug de template é válido sem verificação explícita.

## Contexto
Aplicar sempre que houver carregamento de categorias canônicas ou qualquer estrutura que referencie templates por slug.

## Origem
2025-04-08, commit: observation: categorias_canonicas mismatch silencioso