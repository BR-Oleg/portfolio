# Snippet — RBAC + assign de ticket

Fonte: `backend/server.js`

```js
function authorizeRoles(roles) {
  return (req, res, next) => {
    if (!roles.includes(req.user.role)) {
      return res.status(403).json({ error: "Acesso negado." });
    }
    next();
  };
}

// Agent só enxerga a própria fila
if (req.user.role === "agent") {
  tickets = await Ticket.find({ assignedTo: req.user.id });
}

app.put(
  "/api/tickets/:id/assign",
  authenticateToken,
  authorizeRoles(["user", "admin"]),
  async (req, res) => {
    const { assignedTo } = req.body;
    const updatedTicket = await Ticket.findByIdAndUpdate(
      req.params.id,
      { assignedTo },
      { new: true },
    );
    res.json(updatedTicket);
  },
);
```

**Por que importa:** operação real de mandato — não “todo mundo vê tudo”.
