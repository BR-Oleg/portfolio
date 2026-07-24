# Snippet — check-in anti-fraude + selos

Fonte: `lib/services/gamification_service.dart`

```dart
// Firestore não permite múltiplos range filters na mesma query → filtra em memória
final establishmentCheckIns = await _firestore
    .collection('checkins')
    .where('userId', isEqualTo: userId)
    .where('establishmentId', isEqualTo: establishmentId)
    .get();

final today = establishmentCheckIns.docs.where((doc) {
  final createdAt = DateTime.parse(doc.data()['createdAt'] as String);
  return createdAt.isAfter(startOfDay);
}).toList();
if (today.isNotEmpty) {
  throw Exception('Você já fez check-in neste estabelecimento hoje.');
}

// cooldown 1h entre check-ins (qualquer estabelecimento)
if (minutesSinceLast < 60) {
  throw Exception('Aguarde $minutesRemaining minuto(s) antes de fazer outro check-in.');
}

// Selos: Bronze (1 check-in) → Prata (10 reviews + 5 check-ins + 2 referrals)
// → Ouro (25 reviews + 10 referrals)
```

**Por que importa:** gamificação com fricção anti-farm e filtro em memória onde a query do Firestore não cobre.
