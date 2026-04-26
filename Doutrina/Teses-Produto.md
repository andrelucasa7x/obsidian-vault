# Teses de Produto — Doutrina NEOTECH

> **Relação com o resto da doutrina:** As 3 teses abaixo orientam a PRIORIDADE de aplicação dos 13 princípios e das 11 camadas. Não substituem nenhuma regra existente. Princípios respondem "como fazer"; teses respondem "o que importa".
>
> **Origem:** Chat Claude.ai 22/abr/2026 — construção empírica do brand_kit NEOTECH
> **Criada em:** 2026-04-22

---

## TESE 14 — Venda é o único KPI que importa

O Social Agent existe pra fazer o cliente vender mais. Tudo que o sistema faz — posts, carrosséis, grid bonito, bio bem escrita, identidade visual, calendário editorial, tráfego pago — é meio. O fim é venda.

**Critério binário:**
- Cliente vendeu mais esse mês por causa do sistema → ✅ produto funcionou
- Cliente teve posts bonitos mas não vendeu → ❌ produto falhou

Engagement / alcance / seguidores são indicadores, não resultados. Todo output precisa rastro `post → cliques → DMs → vendas → receita atribuída`. Engagement sem venda é ruído.

**Implicações operacionais:**
- Quando houver trade-off entre features, priorizar a que mais aproxima do KPI venda
- Pricing precisa ser justificável em venda recuperada em 30 dias; se não, cliente cancela
- Cada departamento é avaliado por contribuição à venda do tenant

---

## TESE 15 — Dogfooding Triplo quebra o cold-start

NEOTECH usa o próprio Social Agent pra vender NEOTECH, vender Social Agent e crescer Nocast. 3 tenants internos cobrem 3 nichos diferentes (portal de notícia, B2B tech, SaaS produto) antes de qualquer cliente externo.

| Tenant | Nicho | Aprendizado que gera | O que prova |
|---|---|---|---|
| Nocast | Portal de notícia | Volume alto, formatos jornalísticos, growth, timing editorial | Funciona pra mídia/infoprodutor |
| Neotech | B2B tech/serviços | Copy técnico, grid modular, conversão DM/call | Funciona pra SaaS/agência/consultoria |
| Social Agent (futuro) | SaaS produto | Tráfego frio, conversão paga, case dogfood | Funciona pra SaaS vendendo a PMEs |

**Loop:** Neotech roda Social Agent pra própria Neotech → @neotech.app cresce → Meta Ads da Neotech vendem Social Agent → leads no WhatsApp → alguns fecham → mais dados → ML melhora → CAC cai.

**Ordem correta de desenvolvimento:**
- Fase 0 (hoje): dogfood manual
- Fase 1: MVP interno (Neotech + Nocast como tenants formais)
- Fase 2: alpha externo (3-5 pagantes manuais, R$ 197)
- Fase 3: beta com landing + pagamento automático
- Fase 4: escala com marketing pago

Reforça o princípio 13 (Anti-cold-start brutal), estendendo do nível técnico (niche-setup em 30s) pro nível de produto (3 tenants internos desde o dia 1).

---

## TESE 16 — ML por tenant é a commodity real, não geração

O que distingue o Social Agent de Canva AI / Midjourney / Jasper / ChatGPT+DALL-E não é gerar output. É **aprender o gosto do cliente em tempo real e aplicar em toda geração futura**. O `learning_profile.json` por tenant é o ativo competitivo.

| Ferramenta | Gera output? | Aprende cliente? |
|---|---|---|
| Canva AI / Midjourney / Jasper / ChatGPT+DALL-E | ✅ | ❌ |
| **Social Agent** | ✅ | ✅ |

A segunda coluna é o moat. Concorrente com orçamento alto iguala a primeira em 6 meses. A segunda exige arquitetura multi-tenant com persistência por cliente — decisão de design desde o dia 1, não recurso que se adiciona depois.

**Implementação prática NÃO começa com fine-tuning de LLM** (caro, lento, instável). Começa com:
- Banco de features por tenant (ângulos, horários, CTAs, cores que convertem)
- Prompt injection contextual (toda geração recebe top 5 padrões do tenant)
- Reinforcement loop simples (aprovado + gerou venda = reforço; rejeitado ou sem venda = negativo)

Fine-tuning só com 10+ clientes por nicho e massa crítica de dados. Antes: simplicidade > sofisticação.

Reforça princípio 3 (RLHF passivo > coleta explícita) e princípio 11 (Departamentos virtuais > monolitos). Expande pra arquitetura de dados: estado por tenant, não global.

---

## Mantras que emergem das 3 teses

- "Venda é o único KPI"
- "Dogfood primeiro, escala depois"
- "Aprender cliente > gerar output"

---

## Referências cruzadas

- Princípios 3, 11, 13: ver `Principios.md`
- 11 camadas: ver `Camadas.md`
- Departamentos: ver `Departamentos-Claude-AI.md`
- Fonte da verdade: ver `Fonte-da-Verdade.md`
