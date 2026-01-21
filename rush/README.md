# RUSH

App de running gamificada para el campus de la Universidad de Montemorelos.

## Stack Tecnologico

| Tecnologia | Uso |
|------------|-----|
| Flutter 3.9+ | Framework UI |
| Dart | Lenguaje |
| Google Maps Flutter | Mapas y rutas |
| Geolocator | Tracking GPS |
| sqflite | Base de datos local |
| Provider | State management |

## Estructura del Proyecto

```
lib/
├── models/
│   ├── user.dart
│   ├── run.dart
│   ├── achievement.dart
│   ├── poi.dart
│   └── mission.dart
├── screens/
│   ├── dashboard_screen.dart
│   ├── map_screen.dart
│   ├── profile_screen.dart
│   ├── leaderboard_screen.dart
│   └── history_screen.dart
├── widgets/
│   ├── xp_bar.dart
│   ├── mission_card.dart
│   ├── poi_marker.dart
│   └── run_stats_panel.dart
├── services/
│   ├── location_service.dart
│   ├── database_service.dart
│   └── gamification_service.dart
└── utils/
    ├── constants.dart
    └── formatters.dart
```

## Base de Datos (SQLite)

### Tablas

```sql
-- Usuario
CREATE TABLE users (
    id INTEGER PRIMARY KEY,
    name TEXT,
    faculty TEXT,
    semester INTEGER,
    xp INTEGER DEFAULT 0,
    level INTEGER DEFAULT 1,
    created_at DATETIME
);

-- Carreras
CREATE TABLE runs (
    id INTEGER PRIMARY KEY,
    user_id INTEGER,
    distance REAL,
    duration INTEGER,
    avg_pace REAL,
    route TEXT,  -- JSON de coordenadas
    xp_earned INTEGER,
    created_at DATETIME
);

-- Logros desbloqueados
CREATE TABLE achievements (
    id INTEGER PRIMARY KEY,
    user_id INTEGER,
    achievement_id TEXT,
    unlocked_at DATETIME
);

-- POIs visitados
CREATE TABLE pois_visited (
    id INTEGER PRIMARY KEY,
    user_id INTEGER,
    poi_id TEXT,
    visited_at DATETIME
);

-- Misiones completadas
CREATE TABLE missions_completed (
    id INTEGER PRIMARY KEY,
    user_id INTEGER,
    mission_id TEXT,
    completed_at DATETIME
);
```

## Instalacion

```bash
# Clonar repositorio
git clone <repo-url>
cd rush

# Instalar dependencias
flutter pub get

# Ejecutar
flutter run
```

## Dependencias

```yaml
dependencies:
  flutter:
    sdk: flutter
  google_maps_flutter: ^2.5.0
  geolocator: ^10.1.0
  sqflite: ^2.3.0
  provider: ^6.1.0
  path_provider: ^2.1.0
  intl: ^0.18.0
```

## Configuracion

### Google Maps API

1. Obtener API key en [Google Cloud Console](https://console.cloud.google.com/)
2. Habilitar Maps SDK for Android/iOS

**Android** (`android/app/src/main/AndroidManifest.xml`):
```xml
<meta-data
    android:name="com.google.android.geo.API_KEY"
    android:value="TU_API_KEY"/>
```

**iOS** (`ios/Runner/AppDelegate.swift`):
```swift
GMSServices.provideAPIKey("TU_API_KEY")
```

### Permisos de Ubicacion

**Android** (`android/app/src/main/AndroidManifest.xml`):
```xml
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>
<uses-permission android:name="android.permission.ACCESS_BACKGROUND_LOCATION"/>
```

**iOS** (`ios/Runner/Info.plist`):
```xml
<key>NSLocationWhenInUseUsageDescription</key>
<string>Necesitamos tu ubicacion para trackear tu carrera</string>
<key>NSLocationAlwaysUsageDescription</key>
<string>Necesitamos tu ubicacion para trackear tu carrera en segundo plano</string>
```

## Arquitectura

```
┌─────────────────────────────────────────┐
│              UI (Screens)               │
├─────────────────────────────────────────┤
│              Widgets                    │
├─────────────────────────────────────────┤
│         Provider (State)                │
├─────────────────────────────────────────┤
│             Services                    │
│  ┌──────────┬──────────┬──────────┐    │
│  │ Location │ Database │ Gamific. │    │
│  └──────────┴──────────┴──────────┘    │
├─────────────────────────────────────────┤
│           SQLite / GPS                  │
└─────────────────────────────────────────┘
```

## Comandos Utiles

```bash
# Build Android APK
flutter build apk --release

# Build iOS
flutter build ios --release

# Analizar codigo
flutter analyze

# Tests
flutter test

# Limpiar cache
flutter clean && flutter pub get
```