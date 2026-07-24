# Arquitetura — AtendPolitiq (profundidade)

```mermaid
flowchart TB
  WA[WhatsApp / chatbot] -->|history| API[server.js]
  API --> Parse[processHistory]
  Parse --> Topic[keywords tema]
  Parse --> Sent[léxico sentimento]
  Parse --> Appr[frases aprovação]
  API --> Norm[normalizeBairro atlas]
  Norm --> DB[(Mongo Ticket)]
  DB --> Agg[aggregations dashboard]
  DB --> Map[GeoJSON + BairroSummary]
  Map --> OpenAI[resumo IA]
  OpenAI -->|fallback| Local[últimas falas]
  UI[React/Vite] --> API
  API --> RBAC[roles + assign]
```

## Peças

- `keywords.js` — taxonomia política local
- `bairros.js` + `dqdecaxias.geojson` — território
- `BairroSummary` — cache de inteligência por bairro
- RBAC `superadmin/admin/user/agent`
