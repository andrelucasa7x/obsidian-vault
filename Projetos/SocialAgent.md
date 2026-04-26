---
status: ATIVO
---
# SocialAgent (NOCAST ESTUDIOS)

> Status: **ATIVO — em producao**

---

## O que e
Plataforma SaaS multi-tenant de **marketing autonomo para portais de noticia**. Uma redacao inteira automatizada que so precisa de um "1" no WhatsApp pra funcionar.

## Stack
- **Frontend**: Next.js 14 + React 18 + TypeScript + Tailwind
- **Backend**: Supabase (edge functions, crons, auth, storage)
- **Workers**: GCP Cloud Run (render, ML, RSS)
- **Deploy**: Vercel (auto-deploy via GitHub)

## Cliente Atual
**Nocast** (nocast.com.br) — portal do jornalista [[../Pessoas/Claudionor|Claudionor]] (64 anos)
- Instagram: @nocast.oficial (6.765 seguidores)
- Tenant ID: `de274ffb-abe0-41de-9f4e-7c9ac49a68a4`

## 10 Templates Manus (sagrados)

| Slug | Uso |
|------|-----|
| manus_hero_full | Noticia principal |
| manus_split_side | Foto lateral |
| manus_cinema_top | Foto topo |
| manus_bold_type | Texto destaque |
| manus_frame_box | Frame borda |
| manus_tech_metrics | Dados/numeros |
| manus_tech_launch | Lancamentos |
| manus_glass_card | Efeito glass |
| manus_minimal_line | Minimalista |
| manus_breaking_alert | Breaking news |

Templates sagrados — renderer so preenche slots, nunca muda estrutura.

## Trajetoria
1. **Hoje** — Copiloto. Supervisao humana via WhatsApp.
2. **Proximo** — taste-learner fecha loop. `auto_mode = true`. Publica sozinho.
3. **Futuro** — Agents pedem criativos, sobem campanha paga, A/B test.
4. **Destino** — Agencia de marketing autonoma completa. Multi-tenant.

## Repo e Local
- GitHub: `andrelucasa7x/social-agent` (privado)
- Local: `/mnt/projects/social-agent/`
- Vercel: nocast-social.vercel.app

---
Ver tambem: [[../SocialAgent/Estado-Atual|Estado Atual]] | [[../Arquitetura/Pipeline|Pipeline]]
