# Problemas da V1 (NÃO repetir)

1. **33 agents passivos** — geravam relatórios que ninguém lia
2. **training_label não existia** — feedback do André era perdido
3. **Colunas erradas** — queries retornavam zero dados
4. **design_knowledge com lixo** — 4.960 registros de Pinterest errados
5. **Vertex AI modelo errado** — "gemini-2.0-pro" não existe
6. **NanoBanana desconectado** — endpoint 404
7. **Deploy nunca feito** — Cloud Run rodava versão antiga

**Taxa de aprovação REAL na V1: 4%** (sistema mostrava 95% FALSO)

## V2 corrigiu tudo
- Backend do zero (NADA copiado)
- 12 tabelas limpas no Supabase
- Agents que AGEM (não geram relatórios)
- GPT-4o como brain + Gemini Flash como mãos

## Relacionado
[[Visao-Produto]] | [[DNA-Visual-NOCAST]]
