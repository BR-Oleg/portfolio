# Snippet — filtros dietéticos + stream Firestore

Fonte: `lib/providers/establishment_provider.dart`

```dart
void _listenToEstablishments() {
  _establishmentsSubscription = FirebaseService.establishmentsStream().listen((list) {
    _establishments = list;
    _applyFilters();
    notifyListeners();
    unawaited(GeofencingService.updateRegions(_establishments));
  });
}

void _applyFilters() {
  _filteredEstablishments = _establishments.where((establishment) {
    if (_selectedFilters.isNotEmpty) {
      final hasAllFilters = _selectedFilters.every(
        (filter) => establishment.dietaryOptions.contains(filter),
      );
      if (!hasAllFilters) return false; // AND, não OR
    }
    // + rating, categoria, distância...
    return true;
  }).toList();
}
```

**Por que importa:** restrição alimentar é conjunção; mapa e geofence reagem ao mesmo estado.
