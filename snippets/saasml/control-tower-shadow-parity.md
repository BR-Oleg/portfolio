# Snippet — Control Tower shadow parity

Fonte: `src/lib/control-tower-shadow-parity.ts`

```ts
export function buildControlTowerShadowParityReport(input: {
  client: { orders: IdRecord[]; inventory: IdRecord[]; products: IdRecord[]; suppliers: IdRecord[]; timelineOrders?: IdRecord[] | null };
  bootstrap: Pick<ControlTowerBootstrapPayload, 'orders' | 'timelineOrders' | 'inventory' | 'products' | 'suppliers' | 'meta'>;
}): ControlTowerShadowParityReport {
  const mismatches: ControlTowerShadowParityMismatch[] = [];
  for (const field of ['orders', 'timelineOrders', 'inventory', 'products', 'suppliers'] as const) {
    const clientIds = collectIds(/* ... */);
    const bootstrapIds = collectIds(/* ... */);
    const same = clientIds.size === bootstrapIds.size
      && [...clientIds].every(id => bootstrapIds.has(id));
    if (!same) {
      const { onlyInClient, onlyInBootstrap } = diffIds(clientIds, bootstrapIds);
      mismatches.push({ field, clientCount: clientIds.size, bootstrapCount: bootstrapIds.size, onlyInClient, onlyInBootstrap });
    }
  }
  return { ok: mismatches.length === 0, mismatches, meta: input.bootstrap.meta };
}
```

**Por que importa:** migração de bootstrap server-side sem “esperar que esteja igual”.
