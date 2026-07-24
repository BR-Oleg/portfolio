# Arquitetura — Prato Seguro (profundidade)

```mermaid
flowchart TB
  User[Consumidor] --> App[Flutter]
  Biz[Estabelecimento] --> App
  App --> Prov[EstablishmentProvider]
  Prov --> FS[(Firestore stream)]
  Prov --> Filtros[AND dietary + distância]
  Prov --> Geo[Geofencing filtrado]
  App --> Game[Gamification anti-fraude]
  App --> Pay[Mercado Pago / IAP]
  Admin[admin-dashboard] --> API[Node]
  API --> FS
  API --> MP[Mercado Pago]
```

## Peças

- Filtro conjuntivo de restrições
- Geofence ≤50 regiões, radius 300m, loitering 60s
- Check-in: 1/dia/estabelecimento + cooldown 1h
- Selos compostos (reviews ∩ check-ins ∩ referrals)
