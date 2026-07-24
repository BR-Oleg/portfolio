# Case Study — AtendPolitiq / AtendSys

- ownership: solo
- repo: privado `BR-Oleg/dashboard_atendpolitiq`

## Resumo

Sistema de operação política sobre WhatsApp: ingestão de histórico de conversa → ticket → classificação de tema/sentimento → agregações de dashboard → inteligência territorial por bairro (GeoJSON + cache de resumo com OpenAI e fallback).

## O que o código realmente faz (não o folder tree)

Backend concentrado em `backend/server.js` + `keywords.js` + `bairros.js` + `dqdecaxias.geojson`.

### Pipeline de um ticket

1. Histórico chega (string multi-linha `user:/assistant:` **ou** JSON)
2. `processHistory` normaliza para mensagens tipadas
3. `capturarTema` via dicionário de keywords (`saúde`, `segurança`, …)
4. `extrairSentimento` por léxico PT-BR (score ±)
5. `extrairAprovacao` conta frases de feedback (“obrigado”, “excelente atendimento”…)
6. `normalizeBairro` remove diacríticos e faz match fuzzy contra atlas local de bairros de Duque de Caxias
7. Persistência Mongo (`Ticket` + `assignedTo`)
8. Resumos por bairro: OpenAI com **fallback determinístico** se a API falhar
9. Dashboard agrega resolvidos/conversas/aprovação/tema-do-dia

→ snippets: [histórico+tema](../snippets/atendpolitiq/history-theme.md) · [bairro+IA](../snippets/atendpolitiq/bairro-ai-summary.md) · [RBAC](../snippets/atendpolitiq/rbac-assign.md)

## Destaques

### Inteligência territorial
- Atlas `bairros.js` com lat/lng por bairro
- Mapa carrega `dqdecaxias.geojson`
- `BairroSummary` como cache (tema dominante + ticketCount + summary)

### RBAC operacional
Roles: `superadmin | admin | user | agent`  
- Agent só lista `assignedTo: req.user.id`  
- Assign restrito a `user|admin`  
- Create/delete users só `superadmin`

### Resiliência de NLP
Tema/sentimento **offline** (barato, previsível) + resumo de bairro **online** com degradação graciosa.

## Decisões

| Decisão | Trade-off |
|---------|-----------|
| Keywords antes de LLM para tema | barato/explicável; menos nuance semântica |
| OpenAI só no resumo territorial | custo controlado; humor do mapa ainda “IA” |
| Normalização de bairro local | robustez a digitação; manutenção do atlas |
| Backend single-file | entrega rápida solo; limite de modularidade |

## Limites (honestos)

- Classificação por keywords ≠ NLP profundo
- Sentimento léxico é aproximação
- Credenciais devem viver só em env (nunca hardcode em produção)
- Escala futura pediria filas e serviços extraídos de `server.js`

## Reflexão

O valor Tech Lead aqui é o **sistema de operação + território**, não um chatbot isolado: ticket, papel, mapa, cache de resumo e fallback.
