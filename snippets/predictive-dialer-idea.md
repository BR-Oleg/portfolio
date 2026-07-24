# Snippet — ideia do preditivo (dialer)

Pseudocódigo da taxa preditiva ajustada por conversão e abandono:

```ts
function calculateOptimalCalls(
  agentsAvailable: number,
  predictiveRatio: number,
  conversionRate: number,
  abandonmentRate: number,
) {
  const adjusted = predictiveRatio * (1 + conversionRate) * (1 - abandonmentRate);
  return Math.ceil(agentsAvailable * adjusted);
}
```

Contexto: [case dialer](../cases/05-dialer-zenith.md).
