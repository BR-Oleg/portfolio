# Snippet — worker de sync com timeout e concorrência

Fonte: `src/lib/order-sync-background-worker.ts`

```ts
async function withTimeout<T>(promise: Promise<T>, timeoutMs: number, label: string): Promise<T> {
  let timeout: ReturnType<typeof setTimeout> | undefined;
  try {
    return await Promise.race([
      promise,
      new Promise<T>((_, reject) => {
        timeout = setTimeout(() => reject(new Error(`${label} excedeu ${timeoutMs}ms`)), timeoutMs);
      }),
    ]);
  } finally {
    if (timeout) clearTimeout(timeout);
  }
}

async function processStoresWithConcurrency(selected: SyncStore[], concurrency: number, perStoreTimeoutMs: number) {
  let nextIndex = 0;
  async function worker() {
    while (nextIndex < selected.length) {
      const store = selected[nextIndex++];
      try {
        await withTimeout(syncStore(store), perStoreTimeoutMs, `sync/fast ${store.id}`);
      } catch (error: any) {
        console.warn('[Order Sync Worker] store failed', { storeId: store.id, error: error?.message });
      }
    }
  }
  await Promise.all(Array.from({ length: concurrency }, () => worker()));
}
```

**Por que importa:** sync multi-loja não pode travar o processo inteiro numa loja lenta.
