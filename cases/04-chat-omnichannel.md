# Omnichannel de atendimento

Plataforma multi-canal para times que precisam unificar WhatsApp e outros canais com **tickets, filas, campanhas e multi-tenant** — frontend de operação em Vue/Quasar.

## O problema

Atendimento em inbox pessoal não escala: falta fila, histórico por ticket, campanha e isolamento entre contas.

## Abordagem

Backend TypeScript com services de auth, tickets, campanhas, WhatsApp/WABA, Instagram, Messenger, Telegram e automações; Postgres com migrations extensas; Redis/Bull; Docker Compose; UI Quasar com tempo real via socket.

## Destaques

- Multi-tenant no modelo de dados
- Adapters por canal atrás da mesma abstração de ticket
- Filas para mensagens e campanhas
- Painel operacional completo (não só “chat demo”)

## Decisões

Modularizar por domínio de serviço (tickets, campanhas, canais) e manter o frontend acoplado ao tempo real do backend — prioridade em operação do time, não em micro-frontends.

## Evolução

Endurecer upgrades das libs de WhatsApp (área que mais muda) e automatizar mais smoke tests de canal.

## Stack

TypeScript · Vue/Quasar · Postgres · Redis/Bull · Docker · Socket.IO
