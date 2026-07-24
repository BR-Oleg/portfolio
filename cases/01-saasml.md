# Case Study — SaaSML (torre de controle operacional)

- status: published
- ownership: solo
- repo: privado (`BR-Oleg/saasml`) — **código não alterado neste engajamento**
- evidência: estrutura GitHub (Next app, `services/`, `sql/`, Docker, Vitest ~51 testes)

## Resumo em 30 segundos

SaaS operacional para contas/lojas com inventário, integrações de marketplace (Mercado Livre / Shopee), ingestão de XML/NF-e, conciliação financeira e painéis de performance — com deploy containerizado e suite de testes.

## Problema

Operar múltiplas lojas/canais gera divergência entre pedidos, estoque, custos, XMLs fiscais e recebíveis. O time precisa de uma torre de controle única, não de planilhas desconectadas.

## Restrições

- Integrações externas com OAuth e quotas
- Dados financeiros/fiscais sensíveis (repo privado)
- Evolução contínua com SQL versionado e releases

## Abordagem

Monólito modular em **Next.js** (App Router) com:

- Camada `src/` (UI, hooks, lib, middleware)
- Serviço auxiliar `services/unimake-dfe-bridge` para NF-e
- Scripts SQL versionados em `sql/` (índices, reconciliação, escopos por loja/conta)
- Observabilidade (`instrumentation` / OpenTelemetry API)
- Redis, Firebase/Firestore e Supabase conforme domínio
- WhatsApp (WPPConnect) e automações de e-mail/XML

## Decisões e trade-offs

### 1. Next.js como núcleo do produto
- **Escolhido:** UI + rotas API no mesmo sistema para velocidade de entrega solo
- **Alternativa:** separar BFF e frontend cedo
- **Consequência:** menos hops no início; disciplina de módulos/SQL para não virar bola de neve

### 2. SQL migrado por arquivos datados
- **Escolhido:** mudanças de modelo/performance como artefatos explícitos
- **Alternativa:** só migrations ORM opacas
- **Consequência:** histórico auditável de índices e regras de reconciliação

### 3. Bridge dedicado para DF-e/NF-e
- **Escolhido:** isolar complexidade fiscal/XML
- **Alternativa:** embutir tudo no app web
- **Consequência:** falhas fiscais não derrubam o restante da torre

### 4. Testes com Vitest desde cedo
- **Escolhido:** `vitest run` no fluxo padrão
- **Alternativa:** só QA manual
- **Consequência:** regressões de domínio menos silenciosas

## Validação

- Suite Vitest no repositório
- Dockerfile + `.dockerignore` + pasta `deploy/`/`release/`
- `.env.example` documentando integrações (sem secrets)

## Resultado

Produto SaaS em evolução ativa (versão de package na família 1.4.x) cobrindo fulfillment, histórico financeiro, marketplaces e NF-e própria/fornecedor.

Métricas de negócio: `não publicadas` (privadas).

## Limites

- Repositório privado — inspeção externa via este case + entrevista
- Dependências de provedores (ML/Shopee/Firebase) impõem modos degradados
- Não afirmar SLA/PCI sem evidência operacional pública

## O que eu faria diferente

- Extrair mais bounded contexts para workers dedicados conforme o volume de reconciliação cresce
- Ampliar contratos OpenAPI públicos internos para onboarding mais rápido de features

## Como inspecionar

- Este case + [arquitetura](../architecture/saasml.md)
- Trechos sanitizados em `snippets/` (quando aplicável)
- Prova pública paralela de full-stack: [PseudoCRMRanking](https://github.com/BR-Oleg/PseudoCRMRanking)
