# Evitar espaços brancos com flexbox

## Descoberta
Para evitar espaços brancos indesejados, use flexbox com flex:1 no conteúdo e flex-shrink:0 nos elementos fixos (sponsor e footer) em vez de position:absolute.

## Regra
Ao posicionar elementos como sponsor e footer em slides, evite usar position:absolute. Prefira flexbox: defina o container como display:flex, flex-direction:column, e aplique flex:1 no conteúdo principal e flex-shrink:0 nos elementos fixos (sponsor, footer). Isso garante que o conteúdo ocupe o espaço disponível e os elementos fixos não criem buracos brancos.

## Contexto
Aplicar em layouts de slides ou páginas onde elementos fixos (como rodapé ou patrocinadores) precisam ficar na parte inferior sem criar espaços vazios quando o conteúdo é curto.

## Origem
2025-03-27, commit fix(jornal-diario): remover espaco branco slides 3-5 (flexbox fix)