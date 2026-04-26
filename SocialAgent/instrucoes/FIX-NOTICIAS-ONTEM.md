<contexto>
Página Notícias do frontend (nocast-social.vercel.app/dashboard/noticias).
Tem abas: Ontem / Hoje / Amanhã. Aba "Hoje" mostra ~9479 itens (todos os dias).
Aba "Ontem" mostra zero. Os dados existem no banco: 4.064 sugestões de 22/04.

Causa provável: filtro de data não considera timezone (BRT = UTC-3) OU
"Hoje" não filtra por data (retorna tudo), e "Ontem" filtra errado.

Tabela: content_suggestions
Campo de data: detected_at (timestamp with time zone, UTC no banco)
Tenant Nocast: de274ffb-abe0-41de-9f4e-7c9ac49a68a4
</contexto>

<escopo>
  <arquivos_permitidos>
    src/app/dashboard/noticias/page.tsx
    src/app/dashboard/noticias/*.tsx
    src/components/noticias/*.tsx
    src/hooks/useNoticias*.ts
    src/lib/queries/noticias*.ts
    src/lib/supabase*.ts
  </arquivos_permitidos>
  <arquivos_proibidos>
    supabase/functions/**
    src/app/dashboard/carrosseis/**
    qualquer arquivo fora de noticias/
  </arquivos_proibidos>
</escopo>

<tarefa>
1. Localizar o arquivo que faz a query de content_suggestions para a página Notícias
   (procurar por "detected_at" ou "content_suggestions" no código)

2. Identificar como as abas Ontem/Hoje/Amanhã filtram por data

3. Corrigir o filtro de data levando em conta timezone BRT (UTC-3):
   - "Hoje" = detected_at entre início do dia BRT e fim do dia BRT
   - "Ontem" = detected_at entre início de ontem BRT e fim de ontem BRT
   
   Implementação correta em TypeScript:
   ```ts
   const getBRTDateRange = (offsetDays: number) => {
     const now = new Date()
     // BRT = UTC-3
     const brtOffset = -3 * 60 * 60 * 1000
     const brtNow = new Date(now.getTime() + brtOffset)
     
     const start = new Date(brtNow)
     start.setUTCDate(start.getUTCDate() + offsetDays)
     start.setUTCHours(0, 0, 0, 0)
     
     const end = new Date(start)
     end.setUTCDate(end.getUTCDate() + 1)
     
     // Converter de volta pra UTC real
     return {
       from: new Date(start.getTime() - brtOffset).toISOString(),
       to: new Date(end.getTime() - brtOffset).toISOString()
     }
   }
   // Uso:
   // Hoje: getBRTDateRange(0)
   // Ontem: getBRTDateRange(-1)
   // Amanhã: getBRTDateRange(1)
   ```

4. Aplicar o filtro na query Supabase:
   ```ts
   .gte('detected_at', range.from)
   .lt('detected_at', range.to)
   ```

5. Verificar que os contadores (NA FILA, CARDS PRONTOS, PUBLICADOS) também
   usam o mesmo filtro de data — se estiverem mostrando totais globais, corrigir.
</tarefa>

<nao_fazer>
  - NÃO tocar em edge functions
  - NÃO alterar estrutura da tabela
  - NÃO mudar outras páginas além de noticias
  - NÃO refatorar componentes que estão funcionando
  - NÃO usar bibliotecas de timezone externas (date-fns-tz etc) — lógica simples como acima
</nao_fazer>

<verificacao>
  # Após deploy, verificar no navegador:
  # 1. Aba "Hoje" deve mostrar ~2374 itens (apenas 23/04)
  # 2. Aba "Ontem" deve mostrar ~4064 itens (apenas 22/04)
  # 3. Os turnos (Madrugada/Manhã/Tarde/Noite) devem refletir horários BRT
  grep -r "detected_at" src/app/dashboard/noticias/
  grep -r "getBRTDateRange\|dateRange\|ontem\|yesterday" src/app/dashboard/noticias/
</verificacao>

<criterio_sucesso>
  - Aba "Ontem" mostra notícias (não zero)
  - Aba "Hoje" mostra apenas notícias de hoje (não acumulado de todos os dias)
  - Contadores refletem o dia selecionado
  - Sem regressão nas outras abas
</criterio_sucesso>

<rollback>
  git checkout HEAD -- src/app/dashboard/noticias/
</rollback>

<apos_concluir>
  Reportar: "Filtro de data corrigido. Ontem: X itens, Hoje: Y itens."
  Próximo: garantir copiloto funcionando para Nocast e Neotech.
</apos_concluir>
