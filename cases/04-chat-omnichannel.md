# Case Study — Omnichannel Chat

- ownership: solo
- repos: privado `BR-Oleg/Chat-main` (e linhagem `atendechat-v5.2.1-main`)

## Resumo

Plataforma de atendimento multi-canal com tickets, filas, campanhas, multi-tenant e frontend de operação (Vue/Quasar).

## Problema

Times de atendimento precisam unificar WhatsApp e outros canais com fila, histórico e automação — não com inbox pessoal.

## Abordagem

- Backend TypeScript com dezenas de services (auth, tickets, campaigns, WhatsApp/WABA, Instagram, Messenger, Telegram, OpenAI…)
- Postgres + migrations numerosas
- Redis/Bull para filas
- Docker Compose
- Frontend Quasar/Vue com tempo real (socket)

## Decisões

1. **Multi-tenant** desde o modelo de dados
2. **Filas** para carga de mensagens/campanhas
3. **Canais atrás de adapters** (WhatsApp/Baileys, etc.)

## Honestidade de origem

A estrutura segue o padrão de plataformas omnichannel maduras do ecossistema. O trabalho solo aqui é **possuir, operar, customizar e evoluir** o sistema completo — não um tutorial mínimo.

## Limites

- Superfície grande ⇒ custo de upgrade/segurança contínuo
- Dependência forte de libs de WhatsApp (quebra frequente)

## Reflexão

Para Tech Lead, o sinal é a capacidade de navegar e endurecer um sistema grande de ponta a ponta, sozinho.
