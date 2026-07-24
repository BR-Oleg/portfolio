# Snippet — geofencing filtrado por dieta

Fonte: `lib/services/geofencing_service.dart`

```dart
static Future<void> updateRegions(List<Establishment> establishments) async {
  final preferredFilters = await _loadUserDietaryFilters();
  final regions = <GeofenceRegion>{};

  for (final e in establishments) {
    if (preferredFilters.isNotEmpty) {
      final options = e.dietaryOptions.toSet();
      if (!options.containsAll(preferredFilters)) continue;
    }
    regions.add(GeofenceRegion.circular(
      id: e.id,
      center: LatLng(e.latitude, e.longitude),
      radius: 300,
      loiteringDelay: 60000,
      data: {'id': e.id, 'name': e.name},
    ));
    if (regions.length >= 50) break; // teto OS/API
  }
  // register regions...
}
```

**Por que importa:** notificação de proximidade só para lugares **comestíveis para aquele usuário**.
