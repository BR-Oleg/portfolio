# Snippet — guarda fiscal ML Full + CEP/IBGE

Fonte: `src/lib/unimake-nfe.ts`

```ts
const logisticType = cleanText(shipment?.logistic_type || order?.shipping?.logistic_type).toLowerCase();

if (logisticType.includes('fulfillment')) {
  return {
    success: false,
    message: 'O Mercado Livre nao permite upload de NF-e externa em envios Full.',
    validation: {
      code: 'own_invoice_full_unsupported',
      retryable: false,
      fields: [{
        element: 'mercadolivre.shipment.logistic_type',
        msg: 'Use o Faturador Mercado Livre para pedidos Full.',
      }],
    },
  };
}

// Marketplace frequentemente manda distrito como cidade; ViaCEP/IBGE corrige município.
if (cepAddress.city && (cepAddress.cityMismatch || !recipient.address.city)) {
  if (cepAddress.cityMismatch && recipient.address.city && !recipient.address.district) {
    recipient.address.district = recipient.address.city;
  }
  recipient.address.city = cepAddress.city;
}
```

**Por que importa:** fiscal + logística real, não “chama API e torce”.
