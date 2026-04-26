# ADD VISION TO FERNANDO VAZ — GPT-4o + EVOLUTION BASE64

> Permite que usuário envie imagem da identidade visual pelo WhatsApp.
> Fernando Vaz baixa imagem via Evolution, invoca GPT-4o Vision,
> extrai cores/tipografia/logo em JSON estruturado, persiste em brand_identity.
>
> Arquitetura B: fetch base64 on-demand via Evolution API (não webhookBase64).
> LLM Vision: GPT-4o com response_format json_object.

---

<contexto>
Estado atual do pipeline Fernando Vaz (v4/v16 pós-fixes):
- whatsapp-webhook v16: LID normalization funcionando
- fernando-vaz v4: conversa texto persistindo 3 campos (portal_niche, 
  portal_tone, portal_differencial)
- fernando-vaz-start v4: brand_identity upsert funcionando
- Smoke test Neotech: step 2 chegou, captou "preto, branco, vermelho" 
  como hex, falta fonte e logo

**Limitação detectada:** sistema só processa texto. Quando usuário envia
PNG da identidade visual pelo WhatsApp, message.conversation vem vazio,
Fernando Vaz não tem input pra processar. Caption (se houver) ignorada.

**Validação empírica:** André enviou print pelo zap; whatsapp_conversations
0 rows nos 3 minutos seguintes. Confirma que image-only messages silenciam.

**Decisões tomadas:**
1. LLM Vision: GPT-4o (OPENAI_API_KEY já em secrets, confirmado via outras 
   edge functions que o usam)
2. Fetch imagem: arquitetura B (on-demand via /chat/getBase64FromMediaMessage 
   da Evolution), não webhookBase64 global
3. Fluxo: imagem → base64 → GPT-4o Vision → JSON estruturado → UPDATE 
   brand_identity (NULL-safe, só preenche campos vazios — NÃO sobrescreve 
   valores já coletados por texto)
4. Resposta no zap: Fernando Vaz confirma em linguagem natural o que 
   absorveu, pede confirmação

**Impacto no sistema:**
- whatsapp-webhook: +25 linhas pra detectar image + fetch base64
- fernando-vaz/index.ts: +80 linhas pra branch Vision
- Nova função helper: extractBrandIdentityFromImage()
- Novo constante: VISION_EXTRACTION_PROMPT

**Payload Evolution esperado para mensagem com imagem:**
```json
{
  "event": "messages.upsert",
  "data": {
    "key": { "remoteJid": "...@lid", "remoteJidAlt": "...", "id": "msgId" },
    "message": {
      "imageMessage": {
        "caption": "texto opcional",
        "mediaKey": "...",
        "url": "https://..."
      }
    }
  }
}
```

**Endpoint Evolution pra baixar:**
POST https://nocastestudios2-evolution-api.qlnc7a.easypanel.host/chat/getBase64FromMediaMessage/neotech-evolution
Body: `{ "message": { key, message } }` (passa o body.data inteiro)
Response: `{ "base64": "...", "mimetype": "image/jpeg" }`
Header: `apikey: A8960432CF2E-4543-B372-FDDDEB662D12`
</contexto>

<escopo>
  <arquivos_permitidos>
    supabase/functions/whatsapp-webhook/index.ts
    supabase/functions/fernando-vaz/index.ts
  </arquivos_permitidos>
  <arquivos_proibidos>
    supabase/functions/fernando-vaz-start/index.ts (não tocar)
    supabase/functions/_shared/* (não tocar)
    schema brand_identity, tenants (não tocar)
    tenant Nocast (de274ffb-abe0-41de-9f4e-7c9ac49a68a4)
    tenant Andre Test FV velho
    tenant Andre Test FV Pós-Fix (94362267-d462-4a47-9f23-62f5c9537771)
      — PRESERVAR pra teste
    agent_procedural_memory, agent_semantic_memory
    Outras edge functions (copiloto-*, publish-post, generate-content, etc)
  </arquivos_proibidos>
</escopo>

<tarefa>

# PASSO 1 — Modificar whatsapp-webhook/index.ts

Após o cálculo de `from` (pós fix LID), antes da invocação do handler,
detectar mensagens de imagem e baixar base64.

## 1.1 Localizar

Procurar onde é calculado `message`:
```typescript
const message = body?.data?.message?.conversation || 
                body?.data?.message?.extendedTextMessage?.text || '';
```

## 1.2 Substituir por bloco expandido

```typescript
// Extrair texto OU detectar imagem
const conversationText = body?.data?.message?.conversation || 
                          body?.data?.message?.extendedTextMessage?.text || '';
const imageMessage = body?.data?.message?.imageMessage || null;

let message = conversationText;
let imageBase64: string | null = null;
let imageCaption: string = '';

if (imageMessage && !conversationText) {
  imageCaption = imageMessage.caption || '';
  
  // Baixar base64 via Evolution API
  try {
    const instance = body?.instance || 'neotech-evolution';
    const evolutionUrl = Deno.env.get('EVOLUTION_API_URL') || 
      'https://nocastestudios2-evolution-api.qlnc7a.easypanel.host';
    const evolutionKey = Deno.env.get('EVOLUTION_API_KEY') || 
      'A8960432CF2E-4543-B372-FDDDEB662D12';
    
    const mediaRes = await fetch(
      `${evolutionUrl}/chat/getBase64FromMediaMessage/${instance}`,
      {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
          'apikey': evolutionKey,
        },
        body: JSON.stringify({
          message: {
            key: body.data.key,
            message: body.data.message,
          },
        }),
      }
    );
    
    if (mediaRes.ok) {
      const mediaData = await mediaRes.json();
      imageBase64 = mediaData.base64 || null;
      // Sinalizar pra handler que é imagem
      message = imageCaption || '[IMAGEM_RECEBIDA]';
    } else {
      console.error('[webhook] getBase64 falhou:', mediaRes.status);
      message = imageCaption || '[IMAGEM_FALHOU_DOWNLOAD]';
    }
  } catch (err) {
    console.error('[webhook] Exception baixando base64:', err);
    message = imageCaption || '[IMAGEM_ERRO]';
  }
}
```

## 1.3 Passar imageBase64 pro handler

Localizar invocação do fernando-vaz handler (procurar `.functions.invoke('fernando-vaz'` ou fetch pra /functions/v1/fernando-vaz). Adicionar `image_base64` no payload:

```typescript
// Dentro do objeto enviado ao fernando-vaz
body: JSON.stringify({
  tenant_id: tenant.id,
  phone: from,
  message,
  session_id,
  image_base64: imageBase64,  // NOVO
  image_caption: imageCaption, // NOVO
})
```

## 1.4 Deploy

```bash
cd ~/socialagent-fv
supabase functions deploy whatsapp-webhook --no-verify-jwt
```

---

# PASSO 2 — Modificar fernando-vaz/index.ts

## 2.1 Adicionar constante VISION_EXTRACTION_PROMPT

No topo do arquivo, após imports, adicionar:

```typescript
const VISION_EXTRACTION_PROMPT = `Você é um analista de identidade visual. 
Analise a imagem fornecida (card/post/material de uma marca) e extraia 
os elementos de identidade visual em JSON estruturado.

Retorne APENAS JSON válido com essa estrutura exata (omita campos que não 
conseguir identificar com confiança):

{
  "colors": {
    "primary": "#RRGGBB",
    "secondary": "#RRGGBB",
    "accent": "#RRGGBB",
    "text": "#RRGGBB",
    "primary_dark": "#RRGGBB"
  },
  "typography": {
    "headline": "Nome Fonte, fallback",
    "body": "Nome Fonte, fallback",
    "weight": "100-900"
  },
  "logo": {
    "initial": "letra principal",
    "text_a": "primeira metade do nome",
    "text_b": "segunda metade do nome"
  },
  "labels": {
    "breaking_a": "palavra 1 de label destaque",
    "breaking_b": "palavra 2 de label destaque"
  },
  "style_profile": {
    "layout": "descrição curta do layout",
    "aesthetic": "descrição curta do estilo",
    "category_style": "como categorias são apresentadas"
  },
  "confidence": 0.0-1.0,
  "notes": "observações relevantes que não cabem nos campos"
}

REGRAS:
- Cores em hex #RRGGBB. Identificar primary (fundo/dominante), 
  accent (destaque/alerta), text (texto principal).
- Fontes: identificar família e peso. Se incerto, usar "Inter, sans-serif".
- Logo: se houver texto "NEOTECH", dividir em neo/tech. Se "NOCAST", 
  em no/cast. Padrão geral: dividir pela metade.
- Se imagem for um post/card de notícia, analisar STYLE dele, 
  não conteúdo editorial.
- NÃO inventar campos. Se não houver logo visível, omitir logo.
- confidence reflete quão clara é a identidade na imagem.
`;
```

## 2.2 Adicionar função helper extractBrandFromImage

Antes da função principal (handler), adicionar:

```typescript
async function extractBrandFromImage(
  base64: string,
  openAiKey: string,
): Promise<{ success: boolean; data?: any; error?: string }> {
  try {
    const response = await fetch('https://api.openai.com/v1/chat/completions', {
      method: 'POST',
      headers: {
        'Authorization': `Bearer ${openAiKey}`,
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({
        model: 'gpt-4o',
        max_tokens: 1200,
        response_format: { type: 'json_object' },
        messages: [
          {
            role: 'user',
            content: [
              { type: 'text', text: VISION_EXTRACTION_PROMPT },
              {
                type: 'image_url',
                image_url: {
                  url: `data:image/jpeg;base64,${base64}`,
                  detail: 'high',
                },
              },
            ],
          },
        ],
      }),
    });

    if (!response.ok) {
      const errText = await response.text();
      return { 
        success: false, 
        error: `OpenAI ${response.status}: ${errText.slice(0, 200)}` 
      };
    }

    const result = await response.json();
    const content = result.choices?.[0]?.message?.content;
    if (!content) {
      return { success: false, error: 'Resposta vazia do GPT-4o' };
    }

    const parsed = JSON.parse(content);
    return { success: true, data: parsed };
  } catch (err) {
    return { success: false, error: String(err) };
  }
}

function mapVisionToBrandIdentity(vision: any): Record<string, any> {
  // Map do JSON Vision pra schema real da tabela.
  // SÓ retorna campos que têm valor. NÃO inclui NULLs/undefined.
  const update: Record<string, any> = {};
  
  if (vision.colors?.primary) update.color_primary = vision.colors.primary;
  if (vision.colors?.secondary) update.color_secondary = vision.colors.secondary;
  if (vision.colors?.accent) update.color_accent = vision.colors.accent;
  if (vision.colors?.text) update.color_text = vision.colors.text;
  if (vision.colors?.primary_dark) update.color_primary_dark = vision.colors.primary_dark;
  
  if (vision.typography?.headline) update.font_headline = vision.typography.headline;
  if (vision.typography?.body) update.font_body = vision.typography.body;
  if (vision.typography?.weight) update.font_weight = vision.typography.weight;
  
  if (vision.logo?.initial) update.logo_initial = vision.logo.initial;
  if (vision.logo?.text_a) update.logo_text_a = vision.logo.text_a;
  if (vision.logo?.text_b) update.logo_text_b = vision.logo.text_b;
  
  if (vision.labels?.breaking_a) update.label_breaking_a = vision.labels.breaking_a;
  if (vision.labels?.breaking_b) update.label_breaking_b = vision.labels.breaking_b;
  
  if (vision.style_profile) update.style_profile = vision.style_profile;
  
  return update;
}

function buildVisionReply(vision: any, persisted: Record<string, any>): string {
  const parts: string[] = ['Absorvi tua identidade visual:'];
  
  if (persisted.color_primary || persisted.color_accent) {
    const cores = [
      persisted.color_primary && `primary ${persisted.color_primary}`,
      persisted.color_accent && `accent ${persisted.color_accent}`,
      persisted.color_text && `text ${persisted.color_text}`,
    ].filter(Boolean).join(', ');
    if (cores) parts.push(`- Cores: ${cores}`);
  }
  
  if (persisted.font_headline) {
    parts.push(`- Fonte: ${persisted.font_headline} (peso ${persisted.font_weight || 'default'})`);
  }
  
  if (persisted.logo_text_a && persisted.logo_text_b) {
    parts.push(`- Logo: ${persisted.logo_text_a}/${persisted.logo_text_b}`);
  }
  
  parts.push('');
  parts.push('Ficou certo? Me corrige se algo tá errado.');
  
  return parts.join('\n');
}
```

## 2.3 Integrar no handler principal

No início do handler principal (antes do bloco que chama LLM text/Gemini), 
adicionar detecção de imagem:

```typescript
// Ler payload
const { tenant_id, phone, message, session_id, image_base64, image_caption } = 
  await req.json();

// BRANCH VISION: se tem imagem, processa antes de LLM texto
if (image_base64) {
  console.log('[fernando-vaz] Processando imagem via GPT-4o Vision');
  
  const openAiKey = Deno.env.get('OPENAI_API_KEY');
  if (!openAiKey) {
    console.error('[fernando-vaz] OPENAI_API_KEY ausente');
    // Fallback: segue fluxo texto com mensagem "recebi imagem mas não consegui processar"
  } else {
    const extraction = await extractBrandFromImage(image_base64, openAiKey);
    
    if (extraction.success && extraction.data) {
      const updates = mapVisionToBrandIdentity(extraction.data);
      
      if (Object.keys(updates).length > 0) {
        // NULL-safe: só preenche campos que ainda estão NULL no banco
        // Buscar estado atual
        const { data: currentBI } = await supabase
          .from('brand_identity')
          .select('*')
          .eq('tenant_id', tenant_id)
          .maybeSingle();
        
        const safeUpdates: Record<string, any> = {};
        for (const [key, value] of Object.entries(updates)) {
          // Só atualiza se campo atual é null OU é style_profile (merge)
          if (!currentBI || currentBI[key] === null || key === 'style_profile') {
            safeUpdates[key] = value;
          }
        }
        
        if (Object.keys(safeUpdates).length > 0) {
          const { error: updateErr } = await supabase
            .from('brand_identity')
            .update(safeUpdates)
            .eq('tenant_id', tenant_id);
          
          if (updateErr) {
            console.error('[fernando-vaz] update brand_identity falhou:', updateErr);
          }
        }
        
        // Logar episódio
        await supabase.from('agent_episodic_memory').insert({
          tenant_id,
          agent_name: 'fernando-vaz',
          event_type: 'vision_extraction',
          payload: {
            vision_raw: extraction.data,
            persisted: safeUpdates,
            confidence: extraction.data.confidence || null,
          },
        });
        
        // Responder via zap
        const reply = buildVisionReply(extraction.data, safeUpdates);
        
        // Salvar na conversa + enviar
        await supabase.from('whatsapp_conversations').insert({
          tenant_id,
          session_id,
          phone,
          direction: 'out',
          message: reply,
          context_type: 'fernando_vaz',
        });
        
        await supabase.from('agent_conversations').insert({
          tenant_id,
          session_id,
          agent_name: 'fernando-vaz',
          direction: 'out',
          message: reply,
        });
        
        // Enviar pela Evolution
        const evoUrl = Deno.env.get('EVOLUTION_API_URL') || 
          'https://nocastestudios2-evolution-api.qlnc7a.easypanel.host';
        const evoKey = Deno.env.get('EVOLUTION_API_KEY') || 
          'A8960432CF2E-4543-B372-FDDDEB662D12';
        const instance = Deno.env.get('EVOLUTION_INSTANCE') || 'neotech-evolution';
        
        await fetch(`${evoUrl}/message/sendText/${instance}`, {
          method: 'POST',
          headers: { 'Content-Type': 'application/json', 'apikey': evoKey },
          body: JSON.stringify({
            number: phone,
            text: reply,
          }),
        });
        
        // Log de sucesso
        await supabase.from('agent_logs').insert({
          tenant_id,
          agent: 'fernando-vaz',
          action: 'vision_extraction',
          status: 'success',
          metadata: { extracted_fields: Object.keys(safeUpdates) },
        });
        
        // Retorna cedo — não chama LLM texto
        return new Response(
          JSON.stringify({ 
            success: true, 
            action: 'vision_extraction',
            extracted: Object.keys(safeUpdates),
          }),
          { headers: { 'Content-Type': 'application/json' } }
        );
      }
    } else {
      console.error('[fernando-vaz] Vision falhou:', extraction.error);
      // Fallback: segue fluxo texto informando
      message = '[USUARIO_ENVIOU_IMAGEM_MAS_EXTRACAO_FALHOU]';
    }
  }
}

// ... resto do handler original (LLM texto) continua ...
```

## 2.4 Deploy

```bash
cd ~/socialagent-fv
supabase functions deploy fernando-vaz --no-verify-jwt
```

---

# PASSO 3 — VALIDAÇÃO (não executar, só documentar)

Depois dos 2 deploys:
1. André envia UMA imagem da identidade Neotech pelo zap
2. Claude.ai monitora via MCP:
   - `whatsapp_conversations` com direction='in' (mesmo que message genérico)
   - `agent_episodic_memory` com event_type='vision_extraction'
   - `brand_identity` com campos visuais preenchidos
   - Resposta do Fernando Vaz no zap com "Absorvi tua identidade..."

Se passar: próximas imagens não sobrescrevem (NULL-safe protege).

</tarefa>

<nao_fazer>
- NÃO alterar fernando-vaz-start (não tocar no bootstrap)
- NÃO alterar _shared/fernando-vaz/* (prompts.ts, kits-catalog.ts)
- NÃO alterar schema brand_identity (constraint UNIQUE + NULL constraints já ok)
- NÃO alterar defaults da tabela
- NÃO mexer nos 14 procedural skills
- NÃO alterar fix LID do webhook (funcional)
- NÃO criar tenant novo de teste
- NÃO rodar curl de validação manual no final do deploy
- NÃO deletar agent_logs, agent_conversations, agent_episodic_memory
- NÃO habilitar webhookBase64 na Evolution (decisão B = on-demand)
- NÃO sobrescrever valores já preenchidos em brand_identity 
  (NULL-safe é mandatório)
- NÃO processar múltiplas imagens numa mensagem (MVP = 1 imagem por vez)
</nao_fazer>

<verificacao>
Pré-deploy:
```bash
# Confirmar OPENAI_API_KEY em secrets (via Dashboard ou:)
supabase secrets list --project-ref erfeiyxfrutreckzpkeb | grep OPENAI_API_KEY
# Deve existir. Se não, André cria no dashboard antes do deploy.
```

Pós-deploy 1 (whatsapp-webhook):
```bash
curl -X POST "https://erfeiyxfrutreckzpkeb.supabase.co/functions/v1/whatsapp-webhook" \
  -H "Content-Type: application/json" \
  -d '{
    "event": "messages.upsert",
    "instance": "neotech-evolution",
    "data": {
      "key": { "remoteJid": "5584996967963@s.whatsapp.net", "fromMe": false, "id": "TEST_NO_IMG" },
      "message": { "conversation": "teste pós deploy vision" }
    }
  }'
# Deve retornar HTTP 200 (comportamento texto não mudou)
```

Pós-deploy 2 (fernando-vaz): sem teste curl. Esperar mensagem real do André.

Validação final (Claude.ai via MCP após André enviar imagem real):
```sql
-- Teve event_type='vision_extraction'?
SELECT event_type, payload, created_at FROM agent_episodic_memory
WHERE tenant_id='94362267-d462-4a47-9f23-62f5c9537771'
  AND event_type='vision_extraction'
ORDER BY created_at DESC LIMIT 1;

-- Campos foram preenchidos?
SELECT color_primary, color_secondary, color_accent, color_text,
       font_headline, font_body, font_weight,
       logo_initial, logo_text_a, logo_text_b,
       style_profile
FROM brand_identity
WHERE tenant_id='94362267-d462-4a47-9f23-62f5c9537771';
```
</verificacao>

<criterio_sucesso>
1. ✓ Ambos deploys ok (webhook v17, fernando-vaz v5)
2. ✓ Bundles sem warnings/errors
3. ✓ Curl de texto ainda funciona (regressão zero)
4. ✓ André envia imagem → webhook baixa base64 sem erro
5. ✓ fernando-vaz invoca GPT-4o, JSON parsing ok
6. ✓ brand_identity recebe UPDATE com campos vazios (não sobrescreve preenchidos)
7. ✓ Resposta "Absorvi tua identidade..." chega no zap
8. ✓ agent_episodic_memory registra event_type='vision_extraction'
9. ✓ Nocast intacto
10. ✓ Tenant Andre Test FV Pós-Fix: cores já preenchidas (#000000, #FF0000, 
   #FFFFFF) NÃO foram sobrescritas pela imagem
</criterio_sucesso>

<rollback>
```bash
cd ~/socialagent-fv
# Reverter fernando-vaz
git checkout HEAD~1 -- supabase/functions/fernando-vaz/index.ts
supabase functions deploy fernando-vaz --no-verify-jwt

# Reverter whatsapp-webhook
git checkout HEAD~1 -- supabase/functions/whatsapp-webhook/index.ts
supabase functions deploy whatsapp-webhook --no-verify-jwt
```

Se não tiver git history (download manual):
- Reverter whatsapp-webhook: remover bloco de detecção de imageMessage,
  voltar para `const message = body?.data?.message?.conversation || ...`
- Reverter fernando-vaz: remover constante VISION_EXTRACTION_PROMPT,
  remover funções extractBrandFromImage + mapVisionToBrandIdentity + 
  buildVisionReply, remover branch "if (image_base64)" no handler.
- Re-deploy ambos.

Dados em agent_episodic_memory com event_type='vision_extraction' podem 
ser mantidos como histórico — não afetam operação.
</rollback>

<apos_concluir>
Se passar em 10/10 critérios:
1. Tag git: STABLE-FERNANDO-VAZ-VISION-2026-04-23
2. Reportar ao André:
   - Campos extraídos da imagem
   - Campos ignorados (já preenchidos antes)
   - Confidence score do GPT-4o
3. Próximos .md na fila (pós-validação):
   - CLEANUP-TENANTS-TESTE-E-CRIAR-NEOTECH-REAL.md
   - REFACTOR-LLM-STACK-KIMI-CLAUDE.md (Fernando Vaz Kimi primary,
     WebDesigner Claude Sonnet 4.6, etc)

Se falhar:
- Ler agent_logs filtrando agent='fernando-vaz' AND action='vision_extraction'
- Ler console logs da edge function via Dashboard Supabase
- Decidir próximo .md baseado no erro específico
</apos_concluir>
