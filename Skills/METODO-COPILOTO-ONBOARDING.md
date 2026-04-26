# Skill — Método Copiloto de Onboarding

**Departamento:** CRIAÇÃO (ou futuro Projeto ONBOARDING quando volume justificar)
**Tipo:** Protocolo de conversa + aprendizado iterativo
**Origem:** Chat Claude.ai 22/abr/2026 — validado empiricamente em 18 turnos (brand_kit NEOTECH)
**Aplicável a:** Todo onboarding de novo tenant no Social Agent

---

## O que é

Método de onboarding onde o Social Agent aprende o gosto do cliente em tempo real durante conversa WhatsApp, em vez de gerar output pronto no final.

Princípio: **sistema propõe → cliente reage → sistema aprende da reação específica → sistema propõe de novo já corrigido.**

Cada ciclo reduz distância entre gosto do cliente e output do sistema. É o diferencial competitivo sobre Canva AI, Midjourney, Jasper — aprende cliente, não só gera output (Tese 16).

---

## Copiloto ≠ Automático

| Modo | Como funciona | Problema |
|---|---|---|
| Automático | Sistema gera tudo, cliente aprova/rejeita no final | Cliente rejeita N vezes, sistema nunca acerta |
| Copiloto | Propõe → reage → aprende → propõe corrigido | ✅ Converge rapidamente |

**Regra:** todo onboarding do Social Agent usa Copiloto. Automático só depois de 10+ ciclos (confidence_level ≥ 0.9).

---

## learning_profile.json por tenant

Arquivo atualizado em tempo real durante onboarding e toda interação posterior:

```json
{
  "design_preferences": {
    "prefers_grid_over_editorial": true | false,
    "toc_aesthetic": "bolo_em_camadas | editorial_livre | outro",
    "hates_overdecoration": true | false,
    "approved_fonts": ["..."],
    "rejected_fonts": ["..."],
    "canonical_padding_px": 72,
    "canonical_canvas": "1080x1080"
  },
  "copy_preferences": {
    "tone": "descrição livre",
    "voice_reference": "fernando_vaz | outro",
    "emoji_in_bio": false,
    "anti_patterns": ["somos bons", "melhores do mercado"],
    "signal_phrases": ["frases que o cliente VAI usar"]
  },
  "semantic_colors": { "hex": "função" },
  "validation_triggers": {
    "strong_approval": ["caralho", "perfeito", "foda"],
    "rejection_signal": ["não gostei", "tá fugindo"]
  },
  "confidence_level": 0.0 - 1.0
}
```

Este JSON é o ativo competitivo (Tese 16 — ver `../Doutrina/Teses-Produto.md`).

---

## Roteiro de conversa WhatsApp (7 etapas)

### Etapa 1 — Abertura (1 mensagem)

> Chegou bem! Sou o assistente da Neotech que vai montar sua identidade visual aqui no WhatsApp mesmo. Em 30 min tá pronto. Começamos?
>
> Primeira pergunta: **me conta em 1 frase o que sua empresa faz**. Pode ser cru, tipo "a gente vende marmita pra escritório". Quanto mais simples, melhor.

Captura: description + public_target implícito.

### Etapa 2 — Referências visuais (2-3 trocas)

> Top. Agora manda 3 a 5 prints de perfis no Instagram que você acha que tem o estilo visual que você quer.

Captura: Analista de Referência processa via Vision → extrai paleta, tipografia inferida, padrão de layout, tom.

Fallback se cliente não souber:
> Sem problema. Me diz 3 marcas (qualquer segmento) que você admira visualmente.

### Etapa 3 — Teste de fontes (2-3 turnos iterativos)

Agente envia grid de 6 fontes com o nome do tenant escrito em cada.

> Qual bate mais com o que você imagina? Me diz o número. Se nenhuma servir, me fala por que (ex: "muito geométrica", "muito delicada") que eu busco outras.

Captura: approved_fonts, rejected_fonts, font_traits_rejected. Iterar até escolha com convicção.

### Etapa 4 — Logo + bio (2 turnos)

> Primeira versão:
> [imagem do logo]
> Bio: "[texto]"
>
> O que achou?

Perguntar dúvidas não-óbvias (ex: "Emoji na bio ou deixa crua?" — default sem emoji pra B2B).

### Etapa 5 — Sistema de grid (4-5 turnos)

Propor Modelo 3 Grid — ver skill `MODELO-3-GRID-MODULAR.md`.

> Seu grid vai seguir um sistema modular — 6 tipos de post dentro da mesma moldura. Te mostro o primeiro tipo:
> [post_01_insight do tenant]

Iterar:
- Aprova com força → locka padrão
- Ajusta → aplica, gera próximo tipo
- Rejeita modelo → propõe Modelo 1 ou 2 alternativo

Após aprovação: gerar 6 tipos em lote + grid 3×3 preview.

### Etapa 6 — Validação do grid 3×3 (1 turno crítico)

> Olha o grid completo. Assim vai ficar seu feed depois de 9 posts no ar:
> [preview_grid_3x3.png]
>
> Como tá?

Etapa mais crítica do onboarding inteiro. Reação aqui define:
- confidence_level ≥ 0.85 → lock do brand_kit
- Iterar mais → ajuste estrutural
- Insatisfação fundamental → cliente pode cancelar

### Etapa 7 — Primeiro post real + go-live

> Fechado. Já tô publicando o primeiro post no @[handle]. Próximos vão seguir calendário editorial — você vê antes ou libero direto?

Captura: preferência de supervisão.

---

## Sinais capturados continuamente

| Sinal | Exemplo | Ação no learning_profile |
|---|---|---|
| Aprovação forte | "caralho", "perfeito", "gozei", "foda" | Lock do padrão, replicar |
| Aprovação condicional | "tá bom", "pode seguir", "ok" | Continua, monitora |
| Rejeição direta | "não gostei", "tá fugindo", "melhora" | Retry com ajuste específico |
| Preferência declarada | "sem emoji", "fonte menor", "mais direto" | Registra como regra futura |
| Traço rejeitado | "muito geométrica", "parece infantil" | Adiciona em rejected_traits |
| Comentário casual | "tenho TOC de grid" | Registra como contexto crítico |
| Silêncio pós-envio | Nenhuma reação por X horas | Pergunta direta: "o que achou?" |

---

## Quando NÃO insistir

1. Máx 3 iterações por etapa. Após 3, escalar pra humano.
2. Cliente pede "pula essa parte" → respeita, registra "cliente usa default".
3. Cliente pede "faz você mesmo" → troca temporária pra Automático com defaults do tenant_type similar.
4. Timeout: 24h sem resposta → reengaja. 72h → escala.

---

## Checklist final do onboarding

- [ ] brand_identity populado
- [ ] learning_profile.json com confidence ≥ 0.75
- [ ] 6 tipos de post gerados e aprovados
- [ ] Grid 3×3 validado com sinal forte
- [ ] Carrossel 6 slides (se justifica)
- [ ] Bio aprovada
- [ ] Canais sociais conectados
- [ ] Preferência de supervisão definida
- [ ] Primeiro post publicado

Todos OK → `tenant.status = 'active'`.

---

## Anti-patterns

❌ Briefing longo em formulário antes de output visual. Cliente cansa.
❌ Gerar tudo sem validação intermediária. 1 rejeição obriga refazer tudo.
❌ Decisões técnicas de cara ("prefere hex"). Fornecer opções visuais.
❌ Ignorar rejeição sem motivo. Perguntar: "O que tá fugindo?"
❌ Avançar sem sinal forte. "Tá bom" não é lock.
❌ Ignorar TOC declarado. Se cliente verbaliza preferência estrutural, vira regra-lei.

---

## Integração com departamentos

```
Agente Onboarding (esta skill)
     ↓
  [Etapas 1-2] → Analista de Referência
     ↓
  [Etapas 3-4] → Departamento CRIAÇÃO (logo + fontes)
     ↓
  [Etapa 5]    → CRIAÇÃO instancia Modelo 3 Grid (ver MODELO-3-GRID-MODULAR.md)
     ↓
  [Etapa 6-7]  → Departamento DISTRIBUIÇÃO publica primeiro post
     ↓
  Departamento de Aprendizado registra learning_profile
```

---

## Alinhamento com doutrina

- Princípio 3 (RLHF passivo): captura implícita a cada turno
- Princípio 10 (Catálogo capabilities): agente escolhe modelo/template conforme cliente reage
- Princípio 13 (Anti-cold-start): 30s de conversa já popula preferências
- Tese 16 (ML por tenant): learning_profile.json nasce aqui — ver `../Doutrina/Teses-Produto.md`
