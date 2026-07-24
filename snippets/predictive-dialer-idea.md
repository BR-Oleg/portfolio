# Snippet sanitizado — ideia do preditivo (dialer)

Ilustração conceitual (não é dump do código proprietário):

```ts
// Pseudocódigo sanitizado: taxa preditiva ajustada por conversão e abandono
function calculateOptimalCalls(agentsAvailable: number, predictiveRatio: number, conversionRate: number, abandonmentRate: number) {
  const adjusted = predictiveRatio * (1 + conversionRate) * (1 - abandonmentRate);
  return Math.ceil(agentsAvailable * adjusted);
}
```

Contexto completo: [case dialer](../cases/05-dialer-zenith.md).
