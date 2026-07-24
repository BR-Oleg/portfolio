# Discador preditivo (dialer-zenith)

Discador automático integrado ao **Asterisk** (AMI/ARI/AGI): campanhas, agentes, filas Redis e interface React em tempo real.

## O problema

Call center precisa originar chamadas com taxa preditiva, controlar abandono e dar visibilidade instantânea ao estado do agente.

## Abordagem

Backend **NestJS** modular (`asterisk`, `campaigns`, `agents`, websocket), Postgres + Redis/Bull, frontend React + shadcn, Docker. A arquitetura está documentada em `ARCHITECTURE.md` no projeto.

Ideia do preditivo: [snippet](../snippets/predictive-dialer-idea.md)

## Destaques

- Integração bidirecional com PBX
- Motor de campanha com ajuste por conversão/abandono
- WebSocket para estado de agente e eventos de chamada
- Separação clara frontend ops × backend telefonia

## Decisões

NestJS pela modularidade/DI em domínio de telefonia; preditivo no servidor (não na UI); containers para reproduzir Asterisk + app.

## Evolução

Validação contínua em troncos reais e métricas de abandono em painel operacional.

## Stack

NestJS · React · Postgres · Redis · Asterisk · Docker
