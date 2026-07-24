# Snippet — contrato de receita Shopee

Fonte: `src/lib/shopee-order-revenue.ts` (sanitizado)

```ts
export function resolveShopeeOrderRevenue(input: ShopeeOrderRevenueInput): ShopeeOrderRevenueBreakdown {
  const revenue = toMoney(input.price);
  const storedGrossAmount = toMoney(input.marketplace_gross_amount);
  // Só trata cascade anúncio→cupons se o sync persistiu gross real.
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

**Por que importa:** evita dashboard “otimista” quando o marketplace não entregou breakdown.
