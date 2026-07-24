# Portfólio de Engenharia — BR-Oleg

Cases técnicos redigidos a partir de projetos **solo** (sem colaboradores).  
Código sensível permanece em repositórios privados; aqui estão narrativa, arquitetura e trechos sanitizados.

## Tese

> Engenheiro full-stack com perfil de Tech Lead: entrego produtos web/mobile e sistemas operacionais de ponta a ponta — marketplaces/SaaS, apps consumer, atendimento e telefonia.

## Projetos em destaque

| Case | Domínio | Stack (alto nível) | Repo |
|------|---------|--------------------|------|
| [SaaSML — torre de controle operacional](cases/01-saasml.md) | Inventário, marketplaces, NF-e, financeiro | Next.js, Firebase/Supabase, Redis, SQL, Docker, Vitest | privado |
| [Prato Seguro](cases/02-safeplate.md) | App de restrições alimentares + admin + pagamentos | Flutter, Firebase, Next/Node, Mapbox | privado |
| [AtendPolitiq / AtendSys](cases/03-dashboard-atendpolitiq.md) | Atendimento WhatsApp + inteligência + mapa | Node, React/Vite, WhatsApp, IA de temas/sentimento | privado |
| [Omnichannel Chat](cases/04-chat-omnichannel.md) | Multi-canal (WhatsApp e outros), tickets, filas | TypeScript, Vue/Quasar, Postgres, Redis/Bull, Docker | privado |
| [Discador preditivo](cases/05-dialer-zenith.md) | Campanhas + Asterisk + agentes | NestJS, React, Postgres, Redis, Docker | privado |
| [PseudoCRMRanking](cases/06-pseudocrm-ranking.md) | Gamificação de vendas (prova pública) | Express, MongoDB, React/Vite | [público](https://github.com/BR-Oleg/PseudoCRMRanking) |

## Arquitetura (C4-lite)

- [SaaSML](architecture/saasml.md)
- [Prato Seguro](architecture/safeplate.md)
- [AtendPolitiq](architecture/atendpolitiq.md)

## Como ler

1. Case (problema → implementações → decisões → limites)
2. Diagrama em `architecture/`
3. Trechos reais sanitizados em `snippets/` (SaaSML, AtendPolitiq, Safeplate)
4. Prova pública clonável: [`PseudoCRMRanking`](https://github.com/BR-Oleg/PseudoCRMRanking)

### Snippets em destaque

| Projeto | Artefato |
|---------|----------|
| SaaSML | [receita Shopee](snippets/saasml/shopee-revenue-contract.md), [snapshot v2](snippets/saasml/history-cost-snapshot.md), [shadow parity](snippets/saasml/control-tower-shadow-parity.md), [sync worker](snippets/saasml/order-sync-worker.md), [NF-e Full](snippets/saasml/nfe-full-guard.md) |
| AtendPolitiq | [tema/histórico](snippets/atendpolitiq/history-theme.md), [bairro+IA](snippets/atendpolitiq/bairro-ai-summary.md), [RBAC](snippets/atendpolitiq/rbac-assign.md) |
| Safeplate | [filtros AND](snippets/safeplate/dietary-filters-stream.md), [geofence](snippets/safeplate/geofencing-dietary.md), [check-in](snippets/safeplate/checkin-antifraud.md) |

## Contato

GitHub: [BR-Oleg](https://github.com/BR-Oleg)
