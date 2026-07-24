# Snippet â€” contrato de receita Shopee

Fonte: `src/lib/shopee-order-revenue.ts` 

```ts
export function resolveShopeeOrderRevenue(input: ShopeeOrderRevenueInput): ShopeeOrderRevenueBreakdown {
  const revenue = toMoney(input.price);
  const storedGrossAmount = toMoney(input.marketplace_gross_amount);
  // SÃ³ trata cascade anÃºncioâ†’cupons se o sync persistiu gross real.
  // Fallback gross=revenue esconderia dado faltante e distorceria margem %.
  const hasRevenueBreakdown = input.isShopee && storedGrossAmount > 0;
  const grossAmount = hasRevenueBreakdown ? storedGrossAmount : revenue;
  const platformDiscount = hasRevenueBreakdown ? Math.max(0, toMoney(input.marketplace_platform_discount)) : 0;
  const sellerDiscount = hasRevenueBreakdown ? Math.max(0, toMoney(input.marketplace_seller_discount)) : 0;
  const rawOther = hasRevenueBreakdown
    ? Math.round((revenue - grossAmount + platformDiscount + sellerDiscount) * 100) / 100
    : 0;
  // ...
  return { revenue, grossAmount, platformDiscount, sellerDiscount, hasRevenueBreakdown, /* ... */ };
}
```

**Por que importa:** evita dashboard â€œotimistaâ€ quando o marketplace nÃ£o entregou breakdown.
