# PartyHub - Contexto del Proyecto

## Qué es

PartyHub es una app Android (Kotlin) que funciona como hub de minijuegos para jugar con amigos de forma presencial. Soporta dos modos: **Local** (un solo dispositivo) y **LAN** (cada jugador en su dispositivo, sin internet).

## Juegos incluidos

| Juego | Mecánica | Jugadores |
|-------|----------|-----------|
| **The Mind** | Cooperativo: ordenar números del 1-100 sin comunicarse | 2-8 |
| **El As** | Baraja española: intercambiar cartas a ciegas, sobrevivir con 3 vidas | 3-8 |

## Stack técnico

- **Lenguaje**: Kotlin
- **UI**: XML + Jetpack Compose (nuevos componentes)
- **Arquitectura**: MVVM + Clean Architecture
- **DI**: Hilt
- **Async**: Coroutines + Flow
- **Red LAN**: TCP (juego) + UDP broadcast (descubrimiento de salas)
- **Serialización**: kotlinx.serialization (JSON)
- **Min SDK**: Android 7.0 (API 24)

## Arquitectura y flujo de datos

```
View (Fragment/Compose) → ViewModel → GameEngine (lógica pura) → StateFlow<GameState>
```

En modo LAN, el ViewModel despacha acciones al `NetworkManager` en lugar del Engine local. El Host tiene la GameEngine "verdadera" (fuente de verdad).

### Componentes clave

- **`GameEngine`** (`feature/[game]/engine/`): Lógica pura del juego. 100% Kotlin, 0% Android. Expone `StateFlow<GameState>`, recibe `GameAction` sellados.
- **`ViewModel`** (`feature/[game]/`): Coordina UI state con GameEngine y NetworkManager.
- **`NetworkManager`** (`network/`): Abstrae TCP/UDP. Roles: Host (Server) vs Client.

### Estructura de paquetes (`com.partyhub`)

```
core/model/       → Data classes inmutables (Player, Room, Card). Sin lógica.
core/base/        → Clases abstractas (BaseViewModel, BaseFragment)
core/di/          → Módulos Hilt
data/repository/  → GameRepository
data/prefs/       → SharedPreferences
feature/hub/      → Pantalla principal
feature/setup/    → Configuración de partida (Local/LAN)
feature/themind/  → Juego The Mind (Fragment, VM, Engine)
feature/elas/     → Juego El As (Fragment, VM, Engine)
network/protocol/ → Mensajes (MessageType, DTOs)
network/sockets/  → SocketHandler
network/discovery/→ NSD/UDP para encontrar salas
```

## Reglas para agentes

- **GameEngine NUNCA debe importar `android.*`** — es lógica pura Kotlin.
- Usar `Dispatchers.Main` para UI y `Dispatchers.IO` para Sockets.
- Los sockets deben cerrarse en `ViewModel.onCleared()` o `Lifecycle.onDestroy()`.
- Protocolo LAN: mensajes JSON sobre TCP. UDP solo para descubrimiento.

## Entregas (asignatura UAM - Desarrollo Apps Móviles 2025-2026)

| Entrega | Peso | Contenido | Estado |
|---------|------|-----------|--------|
| E1 | No evaluable | Diseño + requisitos (presentación) | Completada |
| E2 | 30% | Hub + juegos en modo LOCAL | Pendiente |
| E3 | 70% | Añadir modo LAN + presentación | Pendiente |

## Documentación del proyecto

La documentación de diseño está en carpetas numeradas:

- `00-idea-y-concepto/` → Concepto, visión, referencias y apps similares
- `01-requisitos/` → Requisitos funcionales (RF-01 a RF-22), no funcionales (RNF-01 a RNF-22) y casos de uso (CU-01 a CU-12)
- `02-diseño/` → Arquitectura MVVM, modelo de datos (Kotlin), diseño UI/UX
- `03-diseño-ui/` → Mockups en `vistas/` y prompts de generación en `prompts/`
- `EL AS/` → Prototipo web del juego El As (HTML)
- `ENTREGA_1.md` → Documento de la Entrega 1 con diagramas Mermaid (casos de uso, clases, arquitectura) y prototipos UI
