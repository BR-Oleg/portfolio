# Case Study — AtendPolitiq / AtendSys

- ownership: solo
- repo: privado `BR-Oleg/dashboard_atendpolitiq`

## Resumo

Dashboard de atendimento político via WhatsApp: tickets, temas/sentimento, mapa por bairro, papéis (superadmin/admin/user/agent) e conexão de canal.

## Problema

Mandatos recebem volume alto de demandas cidadãs. Sem centralização, perde-se tema, território e accountability do time.

## Abordagem

- Backend Node (Docker) + frontend React/Vite
- Tickets a partir de conversas WhatsApp
- Análise automática de temas/sentimento e resumo por bairro
- RBAC em API e rotas

## Decisões

1. **Ticket como agregador** da conversa (não chat solto)
2. **RBAC fino** (agent só vê o que é seu)
3. **Mapa territorial** como visão executiva, não só lista

## Validação

- README operacional extenso (perfis, fluxos, módulos)
- Estrutura `backend/` + `frontend/` + Dockerfile

## Limites

- Qualidade da IA de tema/sentimento depende de dados e calibração
- WhatsApp implica risco de desconexão/ban — precisa modo degradado operacional

## Reflexão

O valor está menos no “chatbot” e mais no sistema de operação + inteligência territorial.
