# PseudoCRMRanking — gamificação de vendas

Sistema full-stack aberto para equipes de vendas: login, vendas, ranking, metas, conquistas e dashboard admin.

## O problema

Motivar time comercial com feedback visível (pódio, XP, metas) sem planilha manual.

## O que tem

- Auth JWT (Admin / Colaborador)
- CRUD de vendas e comissões
- Ranking e conquistas
- Dashboard com gráficos
- Seed para demo rápida

## Stack

Express · MongoDB · React/Vite · MUI

## Rodar

Repo: [BR-Oleg/PseudoCRMRanking](https://github.com/BR-Oleg/PseudoCRMRanking)

```bash
cd backend && cp .env.example .env && npm i && npm run seed && npm run dev
cd frontend && cp .env.example .env && npm i && npm run dev
```

## Papel no portfólio

Projeto aberto para o revisor clonar. Os cases mais densos de domínio (SaaSML, Prato Seguro, AtendPolitiq) estão em [portfolio](https://github.com/BR-Oleg/portfolio).
