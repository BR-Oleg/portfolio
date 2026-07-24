# Case Study — Prato Seguro (safeplate)

- ownership: solo
- repo: privado `BR-Oleg/safeplate`

## Resumo

App mobile (Android/iOS) para pessoas com restrições alimentares encontrarem estabelecimentos seguros no mapa, com painel admin, pagamentos e backend de suporte.

## Problema

Quem tem restrição (glúten, lactose, vegano, etc.) não confia em busca genérica. Estabelecimentos querem divulgar opções seguras e atrair esse público.

## Abordagem

- **Flutter/Dart** app (`lib/`) com mapas (Mapbox), Firebase Auth/Firestore/Storage/Messaging
- **Admin** web (React/Next) + API Node/Express
- Pagamentos (Mercado Pago) para planos de empresas
- Scripts/ops de VPS e integração WhatsApp auxiliares no monorepo

## Decisões

1. **Flutter** para um código → duas lojas (Android/iOS)
2. **Firebase** para auth/dados/push com time solo
3. **Admin separado** para operação comercial sem inflar o app

## Validação

- `DOCUMENTACAO.md` / README de produto
- `codemagic.yaml` para pipeline mobile
- Estrutura `test/`, `admin-dashboard/`, `web/`

## Limites

- Secrets de Firebase/Google Services ficam fora do portfólio público
- Qualidade de dados de estabelecimentos depende de curadoria/ops

## Reflexão

Separar app consumer de admin/pagamentos manteve o core mobile enxuto enquanto o negócio crescia.
