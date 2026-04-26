# Infraestrutura Completa

> Atualizado: 21/abr/2026

---

## Mapa Geral

```
                    Andre (Parnamirim/RN)
                         │
            ┌────────────┼────────────┐
            ▼            ▼            ▼
       Claude Code    Claude.ai    Obsidian
       (executor)    (estrategia)  (knowledge)
            │
            ▼
    ┌───────────────────────────────────────┐
    │           Laptop Local                │
    │  Lenovo G50-80 | i7-5500U | 8GB RAM  │
    │                                       │
    │  SSD 120GB (/)      SSD 240GB (/mnt) │
    │  ├── orion-router/  ├── social-agent/ │
    │  └── CLAUDE.md      ├── projetos/     │
    │                     ├── midia/        │
    │                     └── arsenal/      │
    └───────────┬───────────────────────────┘
                │
    ┌───────────┼────────────┬──────────────┐
    ▼           ▼            ▼              ▼
 Supabase    Vercel       GCP           GitHub
 (backend)  (frontend)  (workers)      (codigo)
```

## Supabase (`erfeiyxfrutreckzpkeb`, us-east-2)
- **Plano**: Pro
- **Tabelas**: 64 no schema public
- **Edge Functions**: ~46 ativas
- **Cron Jobs**: ~18 ativos
- **Auth**: Supabase Auth (admin user)
- **Storage**: Supabase Storage (imagens de cards)
- **Dados**: 11.534 suggestions, 984 posts, 18.237 logs

## Vercel
- **Projeto**: `nocast-social`
- **URL**: nocast-social.vercel.app
- **Framework**: Next.js 14 + React 18 + TypeScript + Tailwind
- **Deploy**: Auto-deploy via GitHub (`andrelucasa7x/social-agent`)
- **Build**: ~2min por deploy

## Google Cloud Platform (`design-ai-socialagent`)
- **Regiao**: us-central1
- **Auth**: ADC (neoholdingtech@gmail.com)
- **SA**: vertex-brain@design-ai-socialagent.iam

### Cloud Run Services (7)
| Servico | Funcao |
|---------|--------|
| render-worker | Renderiza cards HTML → PNG (Puppeteer) |
| ml-quality-check | Avalia qualidade visual (EfficientNet-B0) |
| rss-bridge | Proxy RSS anti-CORS |
| nocast-social | Frontend legado (migrado pra Vercel) |
| learning-worker | Worker ML aprendizado |
| goaltech-backend | Backend GoalTech (pausado) |
| goaltech-frontend | Frontend GoalTech (pausado) |

### APIs Habilitadas
- Gemini API (classificador + vision)
- Vertex AI
- Cloud Vision
- Speech-to-Text
- Translation
- Natural Language API
- Text-to-Speech
- Drive API v3 (inoperante — OAuth scope)

## GitHub (`andrelucasa7x`)
| Repo | Status |
|------|--------|
| social-agent | **ATIVO** — produto principal |
| designer-ai-platform | Pausado |
| designer-ai | Pausado |
| clipai | Pausado |
| neoagent-ai-studio | Pausado |
| goaltech | Pausado |
| parnamirim-ride-app | Pausado |
| ai-content-studio | Pausado (2018) |

## Local Only (sem GitHub)
- **orion-router/** — `/home/andre/orion-router/` (privado)

---
Ver tambem: [[Orion-Router]] | [[Pipeline]] | [[Stack-LLM]]
