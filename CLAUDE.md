# PartyHub - Contexto del Proyecto

## Qué es

PartyHub es una app Android (Kotlin) que funciona como hub de minijuegos para jugar con amigos de forma presencial. Soporta dos modos: **Local** (un solo dispositivo) y **LAN** (cada jugador en su dispositivo, sin internet).

## Equipo y Entregas

| | |
|-|-|
| **Equipo** | Rafael Romero Monzón · Diego Illescas Lasa |
| **Asignatura** | DADM – UAM EPS 2025-2026 · Alicia Garrido Peña |

| Entrega | Peso | Contenido | Fecha | Estado |
|---------|------|-----------|-------|--------|
| E1 | — | Diseño + requisitos (presentación) | Completada | ✅ |
| E2 | 30% | Hub + juegos en modo LOCAL | 8 abril 2026 | 🔨 En curso |
| E3 | 70% | Añadir modo LAN + presentación | TBD | ⏳ |

### Entrega 2 — Referencia rápida
- **Rúbrica y normas detalladas**: [`ENTREGA2/NORMAS_ENTREGA2.md`](./ENTREGA2/NORMAS_ENTREGA2.md)
- **Plan de entrega y commits**: [`ENTREGA_2.md`](./ENTREGA_2.md)
- Criterios clave: Entorno (0,70) · Recursos/diseño (1,50) · Escuchadores (0,80) · DataBinding (1,25) · Intenciones/Nav (1,50) · Ciclo vida (1,00) · ViewModel (1,25) · Git (1,00) · General (1,00)

## Juegos incluidos

| Juego | Mecánica | Jugadores |
|-------|----------|-----------|
| **The Mind** | Cooperativo: ordenar números del 1-100 sin comunicarse | 2-8 |
| **El As** | Baraja española: intercambiar cartas a ciegas, sobrevivir con 3 vidas | 3-8 |

## Stack técnico

- **Lenguaje**: Kotlin
- **UI**: XML layouts + ViewBinding/DataBinding (Entrega 2) · Jetpack Compose (Entrega 3)
- **Arquitectura**: MVVM + Clean Architecture
- **DI**: Hilt
- **Async**: Coroutines + Flow
- **Navegación**: Jetpack Navigation Component (NavGraph + SafeArgs)
- **Red LAN** (E3): TCP (juego) + UDP broadcast (descubrimiento)
- **Serialización**: kotlinx.serialization (JSON)
- **Min SDK**: Android 7.0 (API 24)

## Arquitectura y flujo de datos

```
View (Fragment) → ViewModel → GameEngine (lógica pura) → StateFlow<GameState>
```

En modo LAN (E3), el ViewModel despacha acciones al `NetworkManager`. El Host tiene la GameEngine "verdadera" (fuente de verdad).

### Componentes clave

- **`GameEngine`** (`feature/[game]/engine/`): Lógica pura del juego. 100% Kotlin, 0% Android. Expone `StateFlow<GameState>`, recibe `GameAction` sellados.
- **`ViewModel`** (`feature/[game]/`): Coordina UI state con GameEngine. Usa `LiveData` o `StateFlow`.
- **`NetworkManager`** (`network/`, solo E3): Abstrae TCP/UDP. Roles: Host vs Client.

### Estructura de paquetes (`com.partyhub`)

```
core/model/       → Data classes inmutables (Player, Room, Card). Sin lógica.
core/base/        → Clases abstractas (BaseViewModel, BaseFragment)
core/di/          → Módulos Hilt
data/repository/  → GameRepository
data/prefs/       → SharedPreferences
feature/hub/      → Pantalla principal (HubActivity, HubFragment)
feature/setup/    → Configuración de partida
feature/themind/  → Juego The Mind (Activity, Fragments, VM, Engine)
feature/elas/     → Juego El As (Activity, Fragments, VM, Engine)
network/          → (Entrega 3)
```

## Reglas para agentes

- **GameEngine NUNCA debe importar `android.*`** — es lógica pura Kotlin.
- Usar `ViewBinding` siempre. `DataBinding` donde se justifique (vistas que observan datos).
- Los Fragments deben usar `viewLifecycleOwner` para observar LiveData/Flow.
- Guardar estado en `onSaveInstanceState()`. Restaurar en `onCreate(savedInstanceState)`.
- Commits con formato: `tipo(ámbito): descripción` (feat, fix, refactor, style, docs, chore).
- Ramas: `main` ← `develop` ← `feature/*`.

## Documentación del proyecto

| Ruta | Contenido |
|------|-----------|
| `00-idea-y-concepto/` | Concepto, visión, referencias |
| `01-requisitos/` | RF-01..22, RNF-01..22, CU-01..12 |
| `02-diseño/` | Arquitectura MVVM, modelo de datos |
| `03-diseño-ui/` | Mockups en `vistas/` y prompts en `prompts/` |
| `ENTREGA2/` | Normas, rúbrica y material de la Entrega 2 |
| `EL AS/` | Prototipo web del juego El As (HTML) |
| `ENTREGA_1.md` | Documento Entrega 1 (diagramas + prototipos) |
| `ENTREGA_2.md` | Plan de entrega 2 (criterios, commits, arquitectura) |
