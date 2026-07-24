# Snippet — normalização de histórico + tema

Fonte: `backend/server.js` + `backend/keywords.js`

```js
function processHistory(historyRaw) {
  if (Array.isArray(historyRaw)) return historyRaw;
  try {
    const obj = JSON.parse(historyRaw.trim());
    if (Array.isArray(obj)) return obj;
    if (obj && Array.isArray(obj.history)) return obj.history;
  } catch (_) {}
  return historyRaw.trim().split("\n").map((l) => {
    if (l.startsWith("user:")) return { role: "user", content: l.replace("user:", "").trim() };
    if (l.startsWith("assistant:")) return { role: "assistant", content: l.replace("assistant:", "").trim() };
    return { role: "user", content: l.trim() };
  }).filter(Boolean);
}

function capturarTema(historico) {
  const texto = processHistory(historico).map(h => h.content).join(" ").toLowerCase();
  for (const tema in TOPIC_KEYWORDS) {
    for (const kw of TOPIC_KEYWORDS[tema]) {
      if (texto.includes(kw)) return tema;
    }
  }
  return "outros";
}
```

**Por que importa:** aceita formatos heterogêneos do canal e classifica offline/barato.
