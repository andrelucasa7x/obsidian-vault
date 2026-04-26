# CLEANUP DEBUG LOGS FERNANDO VAZ

> Remove os 3 debug logs temporários adicionados hoje durante diagnóstico
> do bug Vision. Mantém todos os logs de produção legítimos.
> 1 arquivo, 3 blocos removidos, 1 deploy.

---

<contexto>
Durante diagnóstico do bug Vision (OpenAI key inválida), foram adicionados
3 debug logs temporários em supabase/functions/fernando-vaz/index.ts:

1. action='debug_entry' — logo após destructure do body
2. action='debug_vision_entered' — início da branch if(image_base64)
3. action='debug_vision_result' — após chamada extractBrandFromImage

Bug já foi diagnosticado (sk-proj-... incorreta) e corrigido (key nova
atualizada no Supabase secrets). Vision funcionou end-to-end confirmado
via smoke test. Identidade Neotech extraída de imagem PNG com sucesso.

Tag estável atual: STABLE-FERNANDO-VAZ-VISION-FULL-2026-04-23 (local,
commit bb02e53, push pendente).
</contexto>

<escopo>
  <arquivos_permitidos>
    supabase/functions/fernando-vaz/index.ts
  </arquivos_permitidos>
  <arquivos_proibidos>
    supabase/functions/whatsapp-webhook/index.ts
    supabase/functions/fernando-vaz-start/index.ts
    supabase/functions/_shared/*
    schema brand_identity, agent_logs
    Qualquer tenant
    Qualquer outra edge function
  </arquivos_proibidos>
</escopo>

<tarefa>
1. Remover bloco debug_entry (após session_id resolvido)
2. Remover bloco debug_vision_entered (início if image_base64)
3. Remover bloco debug_vision_result (após extractBrandFromImage)
4. Deploy: supabase functions deploy fernando-vaz --no-verify-jwt
</tarefa>

<nao_fazer>
- NÃO remover action='vision_extraction'
- NÃO remover console.error existentes
- NÃO remover VISION_EXTRACTION_PROMPT
- NÃO remover extractBrandFromImage/mapVisionToBrandIdentity/buildVisionReply
- NÃO remover branch if(image_base64)
- NÃO tocar whatsapp-webhook
- NÃO deletar registros debug_* antigos
- NÃO mexer secrets
</nao_fazer>

<verificacao>
grep -n "debug_entry|debug_vision_entered|debug_vision_result" → 0 matches
grep -n "vision_extraction|extractBrandFromImage|VISION_EXTRACTION" → matches preservados
</verificacao>

<criterio_sucesso>
1. Deploy v9 ACTIVE
2. Zero warnings
3. 0 matches debug_*
4. vision_extraction preservado
5. Após imagem teste: zero debug_* novos, 1 vision_extraction novo
6. brand_identity preservado
</criterio_sucesso>

<rollback>
git checkout STABLE-FERNANDO-VAZ-VISION-FULL-2026-04-23 -- supabase/functions/fernando-vaz/index.ts
supabase functions deploy fernando-vaz --no-verify-jwt
</rollback>

<apos_concluir>
Tag: STABLE-FERNANDO-VAZ-CLEAN-2026-04-23
Próximo .md: CRIAR-NEOTECH-TENANT-REAL.md
</apos_concluir>
