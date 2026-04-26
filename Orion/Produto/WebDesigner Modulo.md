---
name: WebDesignerAI — módulo dentro do DesignerAI
description: Plano futuro de módulo Web Pages dentro do DesignerAI. Mesmo brain, mesmo DNA, sites únicos por cliente.
type: project
---

# WebDesignerAI — Módulo futuro do DesignerAI

**Decisão (24/mar/2026):** NÃO é produto separado. É módulo dentro do DesignerAI.

**Conceito:** Gerar sites/landing pages únicos por cliente, não genéricos. A IA aprende o gosto do cliente (referências visuais) e gera com o DNA dele.

**Diferencial:** Sites do mercado (Wix AI, v0, Framer AI) geram do modelo pra fora (genérico). O WebDesignerAI gera do cliente pra dentro (único).

**Arquitetura:**
```
DesignerAI (plataforma)
├── Cards & Posts     ← existe
├── Web Pages         ← WebDesignerAI (futuro)
├── Stories/Reels     ← futuro
└── Print             ← futuro
```

**Compartilha com DesignerAI:**
- OR1ON Brain
- Vision Agent (analisa referências web)
- Taste Learner (perfil visual do cliente)
- Brand Guard (rejeita se saiu do DNA)
- Supabase (feedback, design_knowledge, visual_references)
- Feedback loop (aprova/rejeita → refina)

**Fluxo:**
1. Cliente mostra 5 sites que gosta
2. Vision extrai DNA (espaçamento, tipografia, ritmo, cores, estilo de imagem)
3. DNA vira design tokens concretos (CSS vars, não adjetivos)
4. IA gera página com regras do cliente
5. Feedback granular por seção → refina

**Status:** Ideia validada. Implementar quando DesignerAI estiver maduro.

**Why:** Andre identificou que output frontend de IA é sempre genérico. Resolver isso = diferencial real de mercado.

**How to apply:** Quando for hora de implementar, criar como novo módulo no DesignerAI, não repo separado.
