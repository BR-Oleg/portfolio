# Case Study — SaaSML (torre de controle operacional)

- ownership: solo
- repo: privado `BR-Oleg/saasml` (**código do repo não foi alterado**)
- evidência aberta: ~170 módulos em `src/lib`, rotas de domínio em `src/app`, SQL versionado, Vitest, Docker, bridge NF-e

## Resumo

SaaS operacional multi-loja para marketplaces (Mercado Livre / Shopee): sincronização de pedidos, torre de controle, histórico financeiro com *read model* + snapshots versionados, reconciliação de recebíveis, política de settlement com fornecedores/expedição, e emissão própria de NF-e com regras de logística Full e correção de endereço via CEP/IBGE.

## Problema de verdade

Não é “um CRUD de pedidos”. O problema é **consistência operacional** sob APIs externas inconsistentes:

- receita bruta ≠ receita realizada (cupons plataforma/loja)
- custo incompleto (falta embalagem/Flex) pode **inflar margem**
- sync precisa sobreviver timeout/falha parcial por loja
- NF-e própria só faz sentido fora de certos `logistic_type`
- UI da torre e bootstrap server-side podem divergir (shadow parity)

## Mapa do sistema (implementado)

| Área | Onde vive | Papel |
|------|-----------|-------|
| Domínio UI | `src/app/{inventory,sales,history,financial,shopee,mercadoturbo,...}` | Superfícies operacionais |
| Núcleo | `src/lib/*` (~170 TS) | Contratos, workers, fiscal, custos, escopos |
| Fiscal isolado | `services/unimake-dfe-bridge` + `src/lib/unimake-nfe.ts` | Emissão/XML sem poluir o app |
| Dados | `sql/*.sql` | Índices, read models, escopos por loja |
| Qualidade | `*.test.ts` + Vitest | Paridade, receita, reconciliação, boot |

## Destaques técnicos (o que impressiona)

### 1. Contrato único de receita Shopee (anti-mentira de margem)
`resolveShopeeOrderRevenue` só aceita cascade anúncio→cupons→receita quando há **gross real** persistido. Sem gross, não inventa “Venda no anúncio” para maquiar `%`.

→ [snippet](../snippets/saasml/shopee-revenue-contract.md)

### 2. History read model + snapshot versionado
`HISTORY_COST_SNAPSHOT_VERSION = 2` invalida snapshots antigos com classificação/margem errada. Custo incompleto vira aviso explícito (`missingCostParts`).

→ [snippet](../snippets/saasml/history-cost-snapshot.md)

### 3. Shadow parity da Control Tower
Compara IDs de orders/inventory/products/suppliers entre cliente e bootstrap server — detecta drift de carga.

→ [snippet](../snippets/saasml/control-tower-shadow-parity.md)

### 4. Worker de sync com concorrência, timeout e cursor por loja
`order-sync-background-worker`: pool de workers, `withTimeout`, `storeSyncInFlight`, rodízio de stores, rescue de returns pós-compra ML.

→ [snippet](../snippets/saasml/order-sync-worker.md)

### 5. Escopo multi-loja tipado
`StoreScope` discriminado (`all` | `single` | `none`) com guardrails de ação (“escolha uma loja específica”).

### 6. Settlement policy engine
Regras com prioridade, `when` (marketplaces, shipping, SKU prefix, janela), `periodShape`, `due`, branches `supplier|expedition|flex`, modo `shadow|active`.

### 7. NF-e própria com domínio fiscal real
- Bloqueia upload externo em envios **Full**
- Idempotência se documento fiscal já existe no pack ML
- Enriquece cidade/UF/IBGE via CEP quando marketplace manda bairro como cidade
- Fluxo Shopee com retry de upload quando XML autorizou mas PSP/marketplace recusou

→ [snippet](../snippets/saasml/nfe-full-guard.md)

### 8. Reconciliação de recebíveis
Modelo rico: buckets `pending|divergent|scheduled|received`, bases net/gross, composition de pagamento, “a receber hoje”.

## Decisões e trade-offs

| Decisão | Por quê | Consequência |
|---------|---------|--------------|
| Next monólito modular | velocidade solo + um deploy | exige disciplina de `lib/` e SQL |
| Snapshots financeiros versionados | listagens rápidas sem recomputar tudo | precisa bump de versão quando muda contrato |
| Shadow parity | migrar bootstrap sem “fechar os olhos” | custo de instrumentação |
| Bridge NF-e separado | fiscal é outro bounded context | mais uma peça operacional |
| Settlement em shadow antes de active | política de $ não pode ir no escuro | complexidade de regras |

## Validação

- Vitest em módulos críticos (receita, reconciliação, parity, boot, custos)
- SQL datado (performance + guardrails)
- `.env.example` sem secrets; Docker

## Limites (honestos)

- Repo privado: prova profunda via case/snippets/entrevista
- Dependência forte de OAuth/quotas ML/Shopee
- Monólito grande: onboarding mental exige mapa (este documento)

## Reflexão Tech Lead

O diferencial não é “usei Next”. É **não deixar a operação mentir para si mesma**: margem, sync, fiscal e multi-loja com contratos explícitos e testes.
