# 10 Templates Manus

> SAGRADOS — nunca alterar estrutura. Renderer so preenche slots.

---

## Templates Ativos

| # | Slug | Fonte | Uso |
|---|------|-------|-----|
| 1 | `manus_hero_full` | 64px | Noticia principal, hero |
| 2 | `manus_split_side` | 48px | Foto lateral |
| 3 | `manus_cinema_top` | 56px | Foto no topo |
| 4 | `manus_bold_type` | 72px | Texto grande destaque |
| 5 | `manus_frame_box` | 56px | Frame com borda |
| 6 | `manus_tech_metrics` | 56px | Dados e numeros |
| 7 | `manus_tech_launch` | 72px | Lancamentos tech |
| 8 | `manus_glass_card` | 56px | Efeito glassmorphism |
| 9 | `manus_minimal_line` | 58px | Minimalista, clean |
| 10 | `manus_breaking_alert` | 64px | Breaking news (vermelho) |

## Slots disponiveis
```
{{IMAGE_URL}}    — URL da imagem de fundo
{{BADGE}}        — Badge de categoria (1 so)
{{HEADLINE}}     — Titulo principal
{{DESCRIPTION}}  — Subtitulo/descricao
{{FOOTER_LEFT}}  — Footer esquerdo
{{CTA}}          — Call to action
```

## Regras
- Templates foram criados pelo **Manus** — design aprovado, congelado
- Renderer (`renderer.py`) injeta dados via `page.evaluate()` nos slots
- **Nunca modificar HTML/CSS dos templates**
- **Nunca adicionar novos templates** sem aprovacao explicita
- Cada template tem identidade propria — a IA escolhe qual usar

---
Ver tambem: [[DNA-Visual]] | [[../SocialAgent/Estado-Atual|Estado Atual]]
