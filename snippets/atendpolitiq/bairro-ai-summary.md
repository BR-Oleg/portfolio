# Snippet — bairro fuzzy + resumo IA com fallback

Fonte: `backend/server.js`

```js
function normalizeBairro(bairroInput) {
  if (!bairroInput) return "desconhecido";
  const inputNormalized = removeDiacritics(bairroInput).toLowerCase();
  for (const key in bairrosCoords) {
    const keyBase = removeDiacritics(key.split(",")[0]).toLowerCase();
    if (inputNormalized.includes(keyBase) || keyBase.includes(inputNormalized)) {
      return key; // chave canônica do atlas local
    }
  }
  return bairroInput;
}

async function generateAISummary(tickets) {
  // monta texto das conversas do bairro...
  try {
    const response = await axios.post("https://api.openai.com/v1/chat/completions", {
      model: process.env.OPENAI_MODEL_NAME || "gpt-4o-mini-2024-07-18",
      messages: [{ role: "user", content: prompt }],
      max_tokens: 200,
    }, { headers: { Authorization: `Bearer ${process.env.OPENAI_API_KEY}` } });
    return response.data.choices[0].message.content;
  } catch (error) {
    return generateSummaryForBairroFallback(tickets); // últimas falas do cidadão
  }
}
```

**Por que importa:** mapa territorial continua útil mesmo se a LLM cair.
