# Case Study — Discador preditivo (dialer-zenith)

- ownership: solo
- repo: privado `BR-Oleg/dialer-zenith`

## Resumo

Discador automático preditivo integrado a Asterisk (AMI/ARI/AGI), com campanhas, agentes, Redis/Bull e UI React.

## Problema

Call centers precisam originar chamadas com taxa preditiva, controlar abandono e dar visibilidade em tempo real aos agentes.

## Abordagem

- Backend **NestJS** modular (`asterisk`, `campaigns`, `agents`, websocket)
- Postgres + Redis
- Frontend React + shadcn
- Docker; arquitetura documentada em `ARCHITECTURE.md`

## Decisões

1. NestJS por modularidade/DI em domínio de telefonia
2. Algoritmo preditivo ajustado por conversão/abandono
3. WebSocket para estado de agente/chamada

## Limites

- Exige Asterisk/troncos reais para prova E2E
- “Kubernetes ready” é intenção de desenho — validar por ambiente

## Reflexão

Telefonia é um domínio impiedoso: o desenho modular e a doc de arquitetura importam tanto quanto a UI.
