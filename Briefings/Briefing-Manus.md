# Briefing Manus — SocialAgent V2

Briefing enviado pro Manus AI. Foco em resolver o design engine.

## 3 Opções propostas
1. **Parametrizar 25 templates** (data-slot + CSS custom properties)
2. **Design System componentizado** (header, headline, photo, footer como componentes)
3. **Engine de renderização com Pillow/Sharp** (composição por camadas, sem browser)

## Problema principal
O renderer HTML é burro — faz replace por regex. Não entende estrutura do template.

## Arquivo completo
~/Área de trabalho/BRIEFING_MANUS.md
