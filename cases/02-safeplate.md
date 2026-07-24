# Case Study — Prato Seguro (safeplate)

- ownership: solo
- repo: privado `BR-Oleg/safeplate`

## Resumo

Produto multiplataforma: app Flutter (consumer + business), painel admin web, Firebase como backend de dados/auth/push, Mapbox, geofencing com filtros dietéticos, gamificação anti-fraude de check-in, e pagamentos (Mercado Pago / IAP).

## Domínio real

Ajudar pessoas com restrições alimentares a achar estabelecimentos **compatíveis com o conjunto** de restrições (não “qualquer tag”), com confiança social (check-in, review, selos) e camada business (menu, delivery, boost, cupons).

## Arquitetura implementada

```
lib/
  models/        establishment, checkin, seal, menu, delivery, trip…
  providers/     auth, establishment (stream + filtros), cart, feature flags…
  services/      mapbox, geofencing, gamification, mercado_pago, offline, iap…
  screens/       home/mapa, business_*, checkout, leaderboard, offline…
admin-dashboard/ frontend + backend (ops/pagamentos)
```

## Destaques técnicos

### 1. Filtro dietético conjuntivo + tempo real
`EstablishmentProvider` escuta Firestore stream, reaplica filtros e **atualiza geofences** automaticamente.

Restrições selecionadas exigem `contains` de **todas** (`every`), não OR frouxo.

→ [snippet](../snippets/safeplate/dietary-filters-stream.md)

### 2. Geofencing sensível a preferências
Registra até 50 regiões (raio 300m, loitering 60s), **pulando** estabelecimentos que não cobrem as preferências do usuário. Notifica entrada com contexto dietético.

→ [snippet](../snippets/safeplate/geofencing-dietary.md)

### 3. Gamificação com regras anti-abuso
- pontos por ação (check-in/review/referral)
- selos Bronze→Prata→Ouro por thresholds compostos
- check-in: 1×/estabelecimento/dia + cooldown 1h entre check-ins
- workaround explícito à limitação do Firestore (sem múltiplos range filters)

→ [snippet](../snippets/safeplate/checkin-antifraud.md)

### 4. Camada business no mesmo app
Screens de menu, delivery config, boost insights, cupons, onboarding de estabelecimento — não é só “mapa bonito”.

### 5. Ops auxiliares no monorepo
Serviços VPS/WhatsApp e `codemagic.yaml` para build mobile (sinal de ciclo real de release).

## Decisões

| Decisão | Por quê |
|---------|---------|
| Flutter | um código Android/iOS |
| Firebase | auth/dados/push com time solo |
| Geofence filtrado | notificação só quando é relevante à restrição |
| Regras de check-in rígidas | ranking sem farm fácil |

## Limites

- Arquivos de config Firebase no repo exigem higiene de secrets em releases públicas
- Pastas aninhadas (`sitepratoseguro`, cópias de dashboard) aumentam ruído do monorepo
- Geolocalização background depende de permissões OS

## Reflexão

Inovação aqui é o **encaixe**: restrição alimentar ∩ proximidade ∩ engajamento confiável — com código que trata edge cases de Firestore e de abuso.
