# Pipeline de Producao

> Atualizado: 21/abr/2026

---

## Pipeline Principal (NUNCA TOCAR)

```
   489 fontes RSS
        │
        ▼
  ┌──────────────┐
  │ news-curator  │  Coleta 24/7
  └──────┬───────┘
         ▼
  ┌──────────────────┐
  │ score-suggestions │  Ranqueia a cada 2min
  └──────┬───────────┘
         ▼
  ┌────────────────────┐
  │ agent-editor-chefe │  Filtra a cada 10min
  │  v16               │  Gate carrossel por categoria
  └──────┬─────────────┘
         ▼
  ┌──────────────────┐
  │ generate-content  │  Copy + legenda + CTA
  └──────┬───────────┘
         ▼
  ┌────────────────────┐
  │ auto-improve-loop  │  v31 — cruzamento RSS,
  │                    │  Pexels REMOVIDO,
  │                    │  WhatsApp threshold 8.5
  └──────┬─────────────┘
         ▼
  ┌──────────────┐
  │ render-card   │  GCP Cloud Run (Puppeteer)
  └──────┬───────┘
         ▼
  ┌───────────────┐
  │ quality-check  │  GCP Cloud Run (EfficientNet-B0)
  │  v18           │  Scores 8.9-9.8
  └──────┬────────┘
         ▼
  ┌──────────────┐
  │ publish-post  │  IG + FB + WP + WhatsApp
  └──────┬───────┘
         ▼
  ┌──────────────────┐
  │ process-schedule  │  Agenda + broadcast
  └──────────────────┘
```

## Copiloto WhatsApp (paralelo ao pipeline)

```
  copiloto-scheduler v3
  (6h-22h BRT, horario)
        │
        ▼
  Envia top noticias → Claudionor
        │
        ▼
  Claudionor responde "1 3"
        │
        ▼
  whatsapp-webhook v6 → publica 1 e 3
        │
        ▼
  copiloto-urgente v3
  (viral >= 9, a cada 2min)
        │
        ▼
  Alerta imediato → Claudionor
```

## Growth Loop

```
  Publicar
     │
     ▼
  Coletar metricas IG (collect-ig-insights v5)
     │
     ▼
  taste-learner (aprende preferencias)
     │
     ▼
  quality-check ML (EfficientNet-B0)
     │
     ▼
  Ajustar conteudo
     │
     ▼
  Publicar melhor ←── loop continuo
```

Status: **completo com quality data real.** Dados acumulando 7-14 dias.

## Edge Functions Criticas (nao tocar)
- `agent-editor-chefe` v16
- `auto-improve-loop` v31
- `copiloto-scheduler` v3
- `copiloto-urgente` v3
- `whatsapp-webhook` v6
- `publish-post`
- `process-schedule`
- `quality-check` v18

---
Ver tambem: [[Infraestrutura]] | [[../SocialAgent/Estado-Atual|Estado Atual]]
