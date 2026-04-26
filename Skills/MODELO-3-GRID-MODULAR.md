# Skill — Modelo 3 Grid Modular

**Departamento:** CRIAÇÃO
**Tipo:** Pattern reutilizável (identidade visual por tenant)
**Origem:** Chat Claude.ai 22/abr/2026 (brand_kit NEOTECH validado empiricamente)
**Aplicável a:** Tenant B2B tech, SaaS, agência, consultoria ou portal de autoridade

---

## O que é

Sistema de identidade visual para feed Instagram baseado em grid rígido com variação controlada dentro do grid. Cada post é uma "camada de bolo" dentro do mesmo sistema — no grid 3×3, todos compartilham a mesma moldura estrutural (header, linhas finas, footer). Só o miolo central varia.

Referências visuais vivas: @supabase, @railway, @raycast, @resend.

---

## Quando aplicar

✅ Tenant B2B tech/SaaS/agência/consultoria
✅ Tenant com TOC de previsibilidade visual
✅ Tenant que produz variedade de tipos (notícia + insight + case + bastidor)
✅ Tenant que usa Instagram como autoridade + portal de conteúdo

❌ Lifestyle/moda/beleza (editorial livre funciona melhor)
❌ Tenant com 1 tipo de conteúdo só (grid fica limitante)
❌ Feed "artístico" com variação livre

**Alternativas:** Modelo 1 (Portal de Autoridade, @stripe) ou Modelo 2 (Marca-Editorial, @wired).

---

## Especificações canônicas (inviolável após lock)

### Canvas e padding
- Dimensões: 1080×1080 px (feed 1:1)
- Background: cor primária do tenant
- Padding: 72px nas 4 bordas (margem ritual — NÃO MUDAR após definido)
- Device scale factor: 2

### 5 posições canônicas (posições 1, 2, 4, 5 invariáveis)

```
┌─────────────────────────────────────────┐
│ 72px padding                            │
│ [TIPO · NÚMERO]    [CATEGORIA/SÉRIE]    │ ← 1. HEADER
│ ─────────────────────────────────────── │ ← 2. LINHA FINA TOPO
│                                         │
│          [CONTEÚDO CENTRAL]             │ ← 3. CONTEÚDO (VARIA)
│            (centralizado)               │
│                                         │
│ ─────────────────────────────────────── │ ← 4. LINHA FINA BASE
│ @handle                  [logo tenant]  │ ← 5. FOOTER
│ localização                             │
└─────────────────────────────────────────┘
```

### Tipografia

| Elemento | Fonte | Peso | Tamanho | Letter-spacing |
|---|---|---|---|---|
| Headline (display) | Por tenant | 500 | 60-92px | -0.035em |
| Body/Sub | Inter | 400 | 18-22px | -0.005em |
| Tag de tipo | Inter | 600 | 12px | 0.24em uppercase |
| Meta/número | Inter | 600 | 12px | 0.18em |
| Logo | Display do tenant | 500 | 24px | -0.02em |

### Cores semânticas (não decorativas)

| Semântica | Função | Hex referência |
|---|---|---|
| Marca/alerta | Tese, dor, problema, CTA | #d92d20 (vermelho) |
| Ganho/resultado | Métrica positiva, solução | #34c759 (verde) |
| Neutro | Contexto, descrição | branco 55% |
| Informação principal | Texto de headline | branco 100% |
| Separador | Linhas finas | rgba(branco, 0.08) |

**Regra:** cor sem função = não usa.

---

## 6 tipos canônicos

| Tipo | Conteúdo central | Quando usa |
|---|---|---|
| Insight | Headline 92px + sub 22px | Tese, manifesto, posicionamento. Single-post. |
| Notícia | Foto 440px + tag categoria + headline 44px | Notícia comentada. Seletiva. |
| Caso | Client header + problema riscado + resultado gigante + 3 métricas | Cliente real com resultado. Carrossel. |
| Mock | Interface UI (WhatsApp, painel) + headline 40px | Produto em operação. |
| Stat | Número gigante 200px + 3 breakdowns | Um dado que importa. |
| Bastidor | Foto 420px + quote + atribuição | Cultura, equipe, código, operação real. |

---

## Carrossel de 6 slides

Pra casos, análises profundas, tutoriais. Estrutura obrigatória:

| Slide | Função | Cor dominante |
|---|---|---|
| 01 | Capa + hook + client badge + "arraste" | Marca/alerta |
| 02 | Antes: o problema | Alerta |
| 03 | Diagnóstico: insight técnico | Neutro |
| 04 | Solução: como resolveu | Ganho |
| 05 | Resultado: painel + depoimento | Ganho |
| 06 | CTA: próximo passo | Marca/alerta |

Jornada: alerta → análise → alívio → ação.

---

## Regras editoriais de mix no grid

### Distribuição em 20 posts

| Tipo | % |
|---|---|
| Insight | 20% |
| Notícia | 20% |
| Caso | 15% |
| Stat | 15% |
| Mock | 15% |
| Bastidor | 15% |

### Regras de ordem

1. Não postar 2 notícias seguidas
2. Caso com intervalo mínimo de 3 dias
3. Insight em posição de destaque
4. Bastidor + Mock alternados dão ritmo
5. Bastidor não repete em coluna vertical adjacente

### Curadoria de notícia

Posta se atende ≥ 2 critérios:
- Ângulo relevante pro público do tenant
- Tenant tem opinião/comentário
- Novidade técnica real (não hype)
- Gancho pra mostrar o tenant em ação

---

## Validação obrigatória antes do lock

Não locka brand_kit sem teste de grid 3×3.

### Checklist pré-lock

- [ ] 6 tipos singles renderizados
- [ ] 6 slides de carrossel renderizados
- [ ] Grid 3×3 montado com 9 posts (6 tipos + 3 repetições)
- [ ] Grid apresentado ao cliente via WhatsApp
- [ ] Cliente aprovou com sinal forte ("caralho", "perfeito", "é isso")
- [ ] Nenhum ajuste estrutural pendente

---

## Procedimento de instanciação (novo tenant)

1. Coletar: cor primária, cor marca, cor ganho, fonte display, fonte body, logo, handle, localização
2. Instanciar 6 singles + 6 carrossel parametrizados com `{{IMAGE_URL}} {{BADGE}} {{HEADLINE}} {{DESCRIPTION}} {{FOOTER_LEFT}} {{CTA}}`
3. Gerar 9 posts de exemplo
4. Montar grid 3×3 preview
5. Enviar ao cliente via WhatsApp
6. Iterar até aprovação com sinal forte
7. Lockar em `brand_identity` + `tenant_design_preferences`
8. Subir templates pro storage + tabela `templates`
9. Disparar primeiro post via `publish-post`

---

## Assets de referência (tenant NEOTECH, seed visual)

Seed visual validado: `/mnt/user-data/outputs/neotech_grid_final/` (ou pasta equivalente do chat de origem). 6 singles + 6 slides de carrossel + previews 3×3.

---

## Alinhamento com doutrina

- Princípio 5 (Templates como dado, não código): parametrização obrigatória
- Princípio 11 (Departamentos virtuais): skill reutilizável por qualquer agent
- Tese 16 (ML por tenant): grid é estrutura base; conteúdo vem do learning_profile
- Ver `../Doutrina/Teses-Produto.md`
