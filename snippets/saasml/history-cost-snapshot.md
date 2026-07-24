# Snippet — history cost snapshot versionado

Fonte: `src/lib/history-read-model.ts`

```ts
export const HISTORY_COST_SNAPSHOT_VERSION = 2;

export function normalizeHistoryCostSnapshot(value: unknown): HistoryCostSnapshot | null {
  if (!value || typeof value !== 'object' || Array.isArray(value)) return null;
  const snapshot = value as HistoryCostSnapshot;
  const version = normalizeNumber(snapshot.version) || 1;
  // Ignora snapshots pré-v2 (classificação Shopee / base de margem errada).
  if (version < HISTORY_COST_SNAPSHOT_VERSION) return null;
  return {
    baseCost: normalizeNumber(snapshot.baseCost),
    packagingCost: normalizeNumber(snapshot.packagingCost),
    flexCost: normalizeNumber(snapshot.flexCost),
    net: normalizeNumber(snapshot.net),
    hasIncompleteCost: Boolean(snapshot.hasIncompleteCost),
    missingCostParts: snapshot.missingCostParts,
    version,
    // ...
  };
}
```

**Por que importa:** read model rápido **com** invalidação explícita quando o contrato financeiro muda.
