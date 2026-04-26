# Embeber assets como base64 para renderizadores serverless

## Descoberta
Renderizadores headless (Chromium) em ambientes serverless não conseguem carregar URLs externas em runtime, exigindo que assets sejam embebidos como base64 no HTML.

## Regra
Ao gerar HTML para renderização em ambientes serverless (ex: Chromium no GCP), sempre converta assets externos (imagens, fontes) para base64 e embeba diretamente no HTML. Nunca confie em URLs externas para serem carregadas em runtime.

## Contexto
Aplicar sempre que o HTML for renderizado por um serviço headless (ex: Puppeteer, Playwright) em ambientes serverless ou restritos.

## Origem
2025-04-14, commit fix(auto-improve-loop): embed image como base64 antes do render-worker (v10)