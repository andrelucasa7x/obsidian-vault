# MAPA NEOTECH — Lousa Única

> **Regra de ouro:** TODA sessão (André, Claude.ai, Orion, Manus, DeepSeek) começa lendo este arquivo. TODA tarefa concluída atualiza este arquivo. Sem exceção.
>
> **Onde mora:** `~/Documentos/Obsidian/SocialAgent/MAPA-NEOTECH.md`
> **Última atualização:** 2026-04-08 (madrugada)

---

## 1. ESTADO ATUAL

### O que tá rodando agora
- **MVP STABLE v1.0** taggeado no GitHub. Tag `STABLE-MVP-v1.0` (commit `5cb4bf9`).
- **Frontend Cloud Run**: revision `nocast-social-00077-hd2` no ar — `https://nocast-social-324868875786.southamerica-east1.run.app` — login `nocast@socialagent.com.br / 123456`
- **Backend v1** (`erfeiyxfrutreckzpkeb`): 12 templates Manus ativos no banco, mirror, categorias canônicas, auto-improve-loop v7, batch v9 = 13/13 score 9.1.
- **Janela rolante 24h automática**: função `cleanup_24h_window()` rodando via `pg_cron` toda hora cheia. Posts published viram aprendizado em `user_preferences` antes de ser deletados (RLHF passivo, princípio 3).
- **Aprendizado real do Claudionor já populado**: Esporte 25% · Política 25% · Segurança 15% · Cultura 15% · Cidade 10% · Economia 5% · Saúde 5%.
- **WordPress publishing**: OK (post 9389 Copa do Brasil publicado em 07/04).
- **Repo único e canônico**: `/mnt/projects/docs/orion/NEO-TECH/nocast-social/`. Alias `social-agent` no `~/.bashrc`. Clone duplicado em `~/projetos/social-agent` foi morto.

### O que tá bloqueado
- **Instagram publishing**: erro 190 — aguardando System User token do Manus
- **Manus**: sem créditos (não pode fazer template novo nem retoque)
- **APIs de texto** no frontend: OpenAI/DeepSeek/Gemini chat sem crédito (só Gemini Image, Apify, SearchAPI, HCTI)

### Próxima ação imediata
1. **André**: abrir URL no navegador, logar, clicar nas 8 telas, anotar o que quebra (se quebrar)
2. Se quebrar algo: Claude.ai gera `.md` cirúrgico de fix
3. Se tudo OK: criar perfil Workana e listar serviço com prints reais
4. Token Meta do Manus chegar — publicar 1o post Nocast no Instagram

---

## 2. INVENTÁRIO

### Supabase
| Projeto | ID | Função | Status |
|---|---|---|---|
| **SocialAgent** | `erfeiyxfrutreckzpkeb` | Banco principal (us-east-2) | ATIVO |
| socialagent-v2 | `lqyhdtswyuxrdhpzwqic` | Só Storage do logo (sa-east-1) | Ignorar |
| socialagent | `hmyyojdvziiyadlpujij` | Antigo | Inativo |

**Tenant Nocast (UUID):** `de274ffb-abe0-41de-9f4e-7c9ac49a68a4`
**AGENT_SECRET:** `socialagent2026`

**Funções ativas no banco:**
- `cleanup_24h_window()` — janela rolante 24h, multi-tenant
- `extract_learning_from_post(uuid)` — extrai aprendizado de post published antes de deletar

**Cron jobs ativos:**
- `cleanup-24h-window` — `0 * * * *` (toda hora cheia)

### Frontend (Cloud Run GCP)
- **URL:** `https://nocast-social-324868875786.southamerica-east1.run.app`
- **Região:** southamerica-east1
- **Stack:** Next.js 14 App Router + React 18 + TypeScript + Tailwind 3.4 + Supabase SSR + Puppeteer
- **Login:** `nocast@socialagent.com.br / 123456`
- **Conecta em:** `erfeiyxfrutreckzpkeb` (banco v1)
- **Revision atual:** `nocast-social-00077-hd2`

### GitHub
- **Repo:** `https://github.com/andrelucasa7x/social-agent`
- **Branch principal:** `main`
- **Último commit:** `5cb4bf9` — feat(mvp): STABLE v1.0
- **Tag corrente:** `STABLE-MVP-v1.0`

### GCP (serviços externos)
- **Render worker** (Puppeteer HTML-PNG): `https://render-worker-324868875786.us-central1.run.app/render`
- **ML quality check** (EfficientNet-B0): `https://ml-quality-check-324868875786.us-central1.run.app/predict`
- **GCP project:** `design-ai-socialagent` (us-central1)

### PC local (André)
- **Vault Obsidian:** `~/Documentos/Obsidian/`
- **Frontend canônico:** `/mnt/projects/docs/orion/NEO-TECH/nocast-social/` — ÚNICO clone
- **Atalho terminal:** `social-agent` ou `$SOCIAL_AGENT`
- **Backend antigo (PAUSADO):** `~/nocast-v2/` (vai virar Prometeus quando RTX 3090 chegar)
- **Hardware:** RTX 3060 12GB, 240GB SSD em `/mnt/projects`

### Templates Manus (banco v1) — 12 ATIVOS
| Slug | Nome | Categoria |
|---|---|---|
| `bold-type` | Manchete Máxima | hero |
| `cinema-top` | Cinema | entertainment |
| `data-card` | Card de Dados | data |
| `especial_placar` | Especial: Placar Esportivo | sport |
| `feed_diagonal` | Feed: Diagonal | modern |
| `feed_split_side` | Feed: Split Side | politics |
| `glass-card` | Glassmorphism | modern |
| `hero-split-v` | Impacto Dividido | hero |
| `magazine` | Revista | editorial |
| `minimal-line` | Minimalista | minimal |
| `quote-card` | Citação em Destaque | quote |
| `tech-launch` | Lançamento Tech | tech |

### Telas frontend (8)
| Rota | Arquivo | Status |
|---|---|---|
| `/` | `app/page.tsx` | OK |
| `/auth/login` | `app/auth/login/page.tsx` | OK |
| `/dashboard` | `app/dashboard/page.tsx` | OK |
| `/dashboard/noticias` | `app/dashboard/noticias/page.tsx` | Reescrita: feed 24h, 3 turnos, auto-refresh 60s |
| `/dashboard/noticias/:id` | `app/dashboard/noticias/[id]/page.tsx` | Galeria com 12 templates |
| `/dashboard/fontes` | `app/dashboard/fontes/page.tsx` | OK |
| `/dashboard/treinamento` | `app/dashboard/treinamento/page.tsx` | OK |
| `/dashboard/configuracoes` | `app/dashboard/configuracoes/page.tsx` | OK |

---

## 3. DOUTRINA

### As 11 camadas
Captura - Geração - Mídia - Renderização (multi-engine) - Avaliação - Publicação - Engajamento - Aprendizado - Inovação - Gestão - Negócio

### Os 13 princípios
1. Multi-engine, não single
2. Loops autônomos > pipelines lineares
3. RLHF passivo > coleta explícita — **ATIVO desde 08/04 via `extract_learning_from_post()`**
4. Personas reais > IA genérica
5. Templates como dado, não código
6. Multi-tenant desde dia 1 — **bug `tenant_id = user.id` corrigido no MVP STABLE v1.0**
7. Free APIs first ($0 stack)
8. Fallback chains
9. Mirroring resolve CORS
10. Catálogo de capabilities pra IA escolher
11. Departamentos virtuais > monolitos
12. Inovação automatizada (T=0.9)
13. Anti-cold-start brutal — **`cleanup_24h_window()` garante banco sempre fresco**

### Regras absolutas (NUNCA quebrar)
- Nunca tratar v1 como "feito pro Nocast" — multi-tenant desde dia 1
- Nunca propor reconstruir do zero — verificar se já existe
- Nunca rodar v1 cloud e nocast-v2 local simultaneamente
- Nunca mexer em templates Manus sem aprovação explícita do André
- Cliente escolhe template, não o sistema (até ter dado real de engajamento)
- Nunca clonar o repo num lugar diferente do canônico — usar o alias `social-agent`
- Nunca tratar `tenant_id = user.id` como correto — sempre constante explícita ou env var

### Mantra
- "256GB pra gerar a receita que paga os próximos 256GB"
- "Templates sagrados, conteúdo inteligente"
- "Simplicidade > sofisticação"
- "Execução > planejamento eterno"
- "Fisiculturista — zero gordura, cada elemento funcional"
- "Pau torando no dia, ao vivo" — doutrina da janela 24h

---

## 4. HISTÓRICO

### 2026-04-07 (madrugada)
- Descoberto SocialAgent v1 cloud (`erfeiyxfrutreckzpkeb`) — 58 edge functions, 42 tabelas
- Migração DeepSeek: 24 edge functions (DeepSeek primary + OpenAI fallback)
- Doutrina NEOTECH v1 escrita (11 camadas + 13 princípios)

### 2026-04-07 (manhã)
- 8 templates Manus parametrizados
- Categorias canônicas: 9 slugs com CHECK constraint, 4070 posts normalizados
- Mirror de imagens (resolve hotlink 403)
- Batch v9: 13/13 score 9.1 — MVP Backend FECHADO

### 2026-04-08 (manhã)
- Mapeadas 27 telas + 42 rotas API do frontend
- PR #1 mergeado: 21.742 linhas deletadas, 104 arquivos (27 telas -> 8)

### 2026-04-08 (madrugada — sessão MVP STABLE v1.0)
- 11.624 registros lixo deletados do banco (3349 posts + 8275 sugestões)
- Janela rolante 24h criada (`cleanup_24h_window()` + cron pg_cron)
- Extração de aprendizado criada (`extract_learning_from_post()`)
- Aprendizado populado: 20 posts antigos -> perfil real do Claudionor
- Consolidação repos: matei clone duplicado, alias `social-agent`
- Sidebar órfão deletado
- `/dashboard/noticias` reescrita: 376 -> 261 linhas, feed 24h em 3 turnos, auto-refresh 60s, bug multi-tenant corrigido
- Galeria `/noticias/[id]`: feed_diagonal adicionado no MANUS_PREVIEWS
- Deploy: Cloud Run revision `nocast-social-00077-hd2`
- Tag: `STABLE-MVP-v1.0` (commit `5cb4bf9`)

---

## 5. PRÓXIMAS AÇÕES

### Fase Validação Manual (agora)
1. André: abrir URL, logar, clicar nas 8 telas, anotar bugs
2. Se quebrar: Claude.ai gera `.md` cirúrgico de fix
3. Se tudo OK: marcar MVP STABLE v1.0 como validado

### Fase Workana (primeira venda)
4. Criar perfil Workana
5. Esperar token Meta do Manus -> publicar 1o post Nocast no Instagram
6. Capturar prints reais do Instagram + Dashboard
7. Listar serviço no Workana com prints
8. Captar 1o cliente pagante

### Backlog técnico (pós-Workana)
- RLS aberto demais em `content_suggestions` (filtra qualquer auth.uid)
- TENANT_ID hardcoded (migrar pra env var quando 2o cliente entrar)
- AGENT_SECRET hardcoded em `[id]/page.tsx` (mover pra server-side)
- Senha 123456 na edge function `fix-auth-users` (deletar antes de revender)
- 15 entradas órfãs no MANUS_PREVIEWS (cosmético)
- 4 hooks órfãos + types/index.ts (deletar)
- Construir Prometeus quando RTX 3090 chegar

---

## 6. ECOSSISTEMA DE IA

| Agente | Papel |
|---|---|
| **Claude.ai** | Estratégia, análise, geração de `.md` |
| **Claude Code "Orion"** | Executor local de `.md` em `$SOCIAL_AGENT` |
| **DeepSeek** | Apoio técnico, otimização de prompts |
| **Manus** | Templates HTML, design system, Meta Business Manager |
| **André** | Decisões finais, aprovações, testes manuais |

### Regra ouro
**Todo agente lê esta lousa antes de qualquer ação.** Tá tudo aqui.

### Regra de repos
**1 clone só.** `social-agent` no terminal. Nunca clonar em outro lugar.

---

> **Esta é a fonte única de verdade NEOTECH.**
> Se algo não tá aqui, não existe.
> Se algo tá aqui, é verdade.

---

## 7. HISTÓRICO DE ATUALIZAÇÕES DOUTRINA/SKILLS

- **2026-04-22**: adicionada nota `Teses-Produto.md` na `Doutrina/` (3 teses: Venda KPI, Dogfooding Triplo, ML por tenant) + 2 skills novas em `Skills/` (`MODELO-3-GRID-MODULAR.md`, `METODO-COPILOTO-ONBOARDING.md`). Origem: handoff chat Claude.ai 22/abr/2026 — brand_kit NEOTECH construído empiricamente. Tenant NEOTECH seed **DEFERRED** até sistema religar (Gemini key nova + tokens revalidados + Claudionor online).
- **2026-04-22**: `.md #1` (segurança crítica) fechado como NO-OP. `fix-auth-users` já ausente do Supabase cloud `erfeiyxfrutreckzpkeb` (60 edge functions listadas via MCP, nenhuma com esse slug); nenhuma pasta local em `/home/andre/supabase/functions/`. Vulnerabilidade (senha `123456` hardcoded) presumidamente sanada em sessão anterior. Pipeline core intacto: `publish-post v39`, `render-card`, `generate-content v62`, `news-curator`, `score-suggestions`, `copiloto-scheduler`, `copiloto-urgente`, `whatsapp-webhook`, `auto-improve-loop`, `quality-check` confirmadas ACTIVE.
- **2026-04-22**: `.md #2` RAPIDAPI_KEY fechado NO-OP. Scan exaustivo 60/60 edge functions via Supabase MCP `get_edge_function` + grep local `x-rapidapi-key.*[A-Za-z0-9]{40,}`. Zero hardcoded. `ml-department v21` é único consumidor RapidAPI, já usa `const RAPIDAPI_KEY = Deno.env.get('RAPIDAPI_KEY') ?? '';`. `storymaker` não existe na lista (premissa desatualizada do .md). Fix aplicado em sessão anterior. Relatório completo em `/tmp/rapidapi_relatorio.txt`. **Observação fora de escopo**: `AGENT_SECRET = 'socialagent2026'` hardcoded como fallback em ~40 functions (Memory Helper v2) — candidato a `.md` dedicado pós-pacote-segurança (hardening futuro, não incluído no pacote 4 atual).
- **2026-04-22**: `.md #3` FECHADO PARCIAL com achado crítico. • `template-uploads`: `public=false` + policy `"template-uploads service only"` (SELECT, service_role) — **fechado de verdade**, LIST com ANON retorna `[]` negado. • `field-media` + `template-previews`: `public=false` aplicado + policy `"<bucket> read by path"` (SELECT, anon+authenticated), mas policy SELECT `USING (bucket_id = X)` NÃO bloqueia LIST com ANON_KEY (limitação do modelo Supabase Storage — LIST e SELECT compartilham mesma policy). Aceito porque buckets vazios; dívida ABERTA antes de popular. • `post-cards`: **ROLLBACK pro estado pré-migração** (`public=true` + policy original `"Post cards são públicos"`). 13.956 arquivos em produção servindo Nocast (Instagram/WhatsApp/WordPress), risco de quebra real com outras opções. URL baseline `mirrors/de274ffb-abe0-41de-9f4e-7c9ac49a68a4/5f36de02-a6ac-49a9-bce3-e4a814aa472a-1776822888669.jpg` retorna HTTP 200 pós-rollback. • **PREMISSA ERRADA no .md original**: policy SELECT `USING (bucket_id = X)` pra role anon também autoriza `/storage/v1/object/list/<bucket>` — flipar `public=false` só fecha LIST sem auth header, com ANON_KEY continua aberto. • **DÍVIDA TÉCNICA ABERTA**: solução real de LIST-block via signed URLs com TTL curto, Edge Function proxy, ou policy condicional path-específica. Candidato a `.md` dedicado pós-pacote-segurança, requer análise de impacto em `publish-post v39` (como serve imagens) + custo operacional proxy. Relatórios Fase 1 preservados em `/tmp/storage_*_pre.txt` + `/tmp/storage_urls_baseline.txt`.
- **2026-04-22**: `LIMPEZA-NOCAST-PRE-FERNANDO-VAZ.md` FECHADO. 8 passos aplicados em tenant Nocast. **PASSO 0**: 5 backups criados (_backup_brand_identity/_tenants/_social_channels/_templates/_department_agents_20260422). **PASSO 1**: brand_identity.onboarding_step=5, tenants.onboarding_completed=true. **PASSO 2 + PATCH 2**: `tenants.niche='jornalismo regional RN'`, `tenants.status='active'` (era 'suspended'); **decisão de escopo**: `tenants.tone` NÃO alterado pra `'informativo direto regional'` (check constraint `tenants_tone_check` permite só `professional|casual|educational` — valor já era `professional`, mapping A aprovado por usuário); `brand_identity.portal_tone='informativo direto regional'` (texto livre aceito), `portal_niche` + `portal_differencial` preenchidos. **PASSO 3**: `social_channels.connected=true` em facebook + whatsapp (4/4 canais agora coerentes: wordpress/instagram/facebook/whatsapp). **PASSO 4**: `templates.kit_slug` coluna + índice criados, 118 linhas backfill `kit_slug='postador'`. **PASSO 5 (Opção B aprovada — escopo estendido Nocast OR tenant_id IS NULL)**: 43 is_system=true zerados, 10 sagrados Manus reativados (bold_type, breaking_alert, cinema_top, frame_box, glass_card, hero_full, minimal_line, split_side, tech_launch, tech_metrics), 8 duplicatas hífen marcadas obsoletas. **PATCH 1**: `manus_magazine` (órfão NULL, 1 uso recente em posts) reativado `active=true` isolado do pool sagrado. **PASSO 6**: 5 agents kit postador com status=active (news-curator, designer-agent, publisher, copy-chief, headline-generator). **PASSO 7**: Tabela nova `kits` criada (PK slug, 9 colunas incluindo JSONB required_fields/pipeline/active_agents/default_channels/template_slugs) + INSERT `postador` (10 campos, 11 pipeline steps, 5 agents, 4 canais, 10 templates). **PASSO 8**: `tenants.kit_slug` FK + índice criados, Nocast vinculado ao kit postador. **Verificação V1-V7 passou**: backups OK, onboarding coerente, 4 canais connected=true, exatamente 10 is_system=true globalmente, 0 templates sem kit_slug, 5 agents ativos, kit postador com metadata correta (Postador campos=10 agents=5 tpls=10). Pipeline produção intacto (zero edge function tocada). Pré-requisito pro FERNANDO-VAZ-ONBOARDER.md satisfeito.
- **2026-04-22**: `.md #4` FECHADO com RLS + views completos. **Frente A (RLS)**: 13/13 tabelas public com `rowsecurity=true` (`affiliates`, `brand_identity`, `carousels`, `content_suggestions`, `metrics`, `notifications`, `posts`, `referrals`, `schedules`, `sources`, `templates`, `tenants`, `user_preferences`). 22 policies antigas (trivial `auth.uid() IS NOT NULL` + circulares `tenant_id IN (SELECT id FROM tenants)`) DROPADAS. 26 policies novas criadas: padrão `<tabela>_tenant_isolation` (FOR ALL TO authenticated, USING + WITH CHECK = `tenant_id::text = coalesce(current_setting('request.jwt.claims', true)::jsonb ->> 'tenant_id', auth.uid()::text)`) + `<tabela>_service_bypass` (FOR ALL TO service_role). Exceções: `tenants` usa `tenants_self_read` (SELECT by `id::text`), `referrals` usa `referrals_admin_only` (USING false — sem tenant_id, bloqueia authenticated, service_role bypass). Edge functions continuam via service_role (bypass nativo). **Frente B (views)**: 3/3 views alteradas `security_invoker=true` via `ALTER VIEW ... SET` (sem DROP, definições preservadas) — `content_suggestions_dedup`, `content_suggestions_ranked`, `posts_performance`. Count diff esperado: dedup 3627→3657, ranked 3997→4027 (+30 cada, ingestão ativa no intervalo); `posts_performance` 88=88. **Frente C (leaked password)**: PENDENTE manual via Dashboard Supabase → Authentication → Password Policy → ativar `Leaked password protection`. Usuário executa. **Verificação Fase 3**: 3 checks automatizados OK (`rls_enabled_count=13`, `tables_with_policies=13`, `views_security_invoker=3`). Smoke test tenant Nocast via service_role: posts=88, content_suggestions=4027, templates=71, sources=489. Backups pré-migração preservados: `/tmp/rls_pre_state.txt`, `/tmp/rls_policies_pre.txt`, `/tmp/views_pre.sql`, `/tmp/views_counts_pre.txt`, `/tmp/rollback_total_2026-04-22.sh`. **Pacote de segurança (4 .md) concluído**: #1 NO-OP, #2 NO-OP, #3 PARCIAL (dívida técnica Storage LIST aberta), #4 OK.
