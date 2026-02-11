# Arquitectura del Sistema

---

## 📐 Visión General

PartyHub sigue el patrón de arquitectura **MVVM** (Model-View-ViewModel) recomendado por Google para aplicaciones Android modernas, con una capa adicional de **Networking** para la comunicación LAN.

---

## 🏗️ Diagrama de Arquitectura

```
┌─────────────────────────────────────────────────────────────────┐
│                        📱 PRESENTATION LAYER                     │
├─────────────────────────────────────────────────────────────────┤
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐              │
│  │   HubView   │  │ TheMindView │  │  ElAsView   │   (Fragments)│
│  └──────┬──────┘  └──────┬──────┘  └──────┬──────┘              │
│         │                │                │                      │
│  ┌──────▼──────┐  ┌──────▼──────┐  ┌──────▼──────┐              │
│  │HubViewModel │  │MindViewModel│  │ AsViewModel │   (ViewModels)│
│  └──────┬──────┘  └──────┬──────┘  └──────┬──────┘              │
└─────────┼────────────────┼────────────────┼─────────────────────┘
          │                │                │
          ▼                ▼                ▼
┌─────────────────────────────────────────────────────────────────┐
│                        🎮 DOMAIN LAYER                           │
├─────────────────────────────────────────────────────────────────┤
│  ┌─────────────────────────────────────────────────────────┐    │
│  │                    Game Engine                           │    │
│  │  ┌─────────────────┐     ┌─────────────────┐            │    │
│  │  │  TheMindEngine  │     │    ElAsEngine   │            │    │
│  │  │  - generateNums │     │  - dealCards    │            │    │
│  │  │  - validatePlay │     │  - handleSwap   │            │    │
│  │  │  - checkGameOver│     │  - resolveRound │            │    │
│  │  └─────────────────┘     └─────────────────┘            │    │
│  └─────────────────────────────────────────────────────────┘    │
│                                                                  │
│  ┌─────────────────────────────────────────────────────────┐    │
│  │                   Use Cases                              │    │
│  │  - StartLocalGame  - StartLANGame  - JoinRoom           │    │
│  │  - PlayCard        - RevealNumber  - EndRound           │    │
│  └─────────────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────────────┘
          │                                    │
          ▼                                    ▼
┌─────────────────────────────┐  ┌────────────────────────────────┐
│      💾 DATA LAYER          │  │      🌐 NETWORK LAYER          │
├─────────────────────────────┤  ├────────────────────────────────┤
│  ┌───────────────────────┐  │  │  ┌────────────────────────┐   │
│  │   PreferencesManager  │  │  │  │    LANManager          │   │
│  │   - volume, vibration │  │  │  │    - createRoom()      │   │
│  │   - playerNames       │  │  │  │    - joinRoom()        │   │
│  └───────────────────────┘  │  │  │    - discoverRooms()   │   │
│                             │  │  │    - sendMessage()     │   │
│  ┌───────────────────────┐  │  │  │    - onMessage()       │   │
│  │    GameRepository     │  │  │  └─────────┬──────────────┘   │
│  │    - saveStats        │  │  │            │                   │
│  │    - getHistory       │  │  │  ┌─────────▼──────────────┐   │
│  └───────────────────────┘  │  │  │    SocketHandler       │   │
│                             │  │  │    - TCP/UDP           │   │
└─────────────────────────────┘  │  └────────────────────────┘   │
                                 └────────────────────────────────┘
```

---

## 📂 Estructura de Paquetes

```
com.partyhub/
├── 📁 app/
│   ├── MainActivity.kt
│   └── PartyHubApplication.kt
│
├── 📁 core/
│   ├── 📁 model/
│   │   ├── Player.kt
│   │   ├── GameState.kt
│   │   ├── GameMode.kt (LOCAL, LAN)
│   │   └── Room.kt
│   ├── 📁 util/
│   │   ├── Extensions.kt
│   │   └── Constants.kt
│   └── 📁 base/
│       ├── BaseViewModel.kt
│       └── BaseFragment.kt
│
├── 📁 data/
│   ├── 📁 preferences/
│   │   └── PreferencesManager.kt
│   └── 📁 repository/
│       └── GameRepository.kt
│
├── 📁 network/
│   ├── LANManager.kt
│   ├── SocketHandler.kt
│   ├── RoomDiscovery.kt
│   └── 📁 protocol/
│       ├── Message.kt
│       └── MessageType.kt
│
├── 📁 feature/
│   ├── 📁 hub/
│   │   ├── HubFragment.kt
│   │   ├── HubViewModel.kt
│   │   └── GameAdapter.kt
│   │
│   ├── 📁 setup/
│   │   ├── SetupFragment.kt
│   │   ├── SetupViewModel.kt
│   │   ├── LocalSetupFragment.kt
│   │   └── LANSetupFragment.kt
│   │
│   ├── 📁 themind/
│   │   ├── TheMindFragment.kt
│   │   ├── TheMindViewModel.kt
│   │   ├── TheMindEngine.kt
│   │   └── 📁 model/
│   │       └── MindGameState.kt
│   │
│   └── 📁 elas/
│       ├── ElAsFragment.kt
│       ├── ElAsViewModel.kt
│       ├── ElAsEngine.kt
│       └── 📁 model/
│           ├── Card.kt
│           └── AsGameState.kt
│
└── 📁 ui/
    ├── 📁 theme/
    │   ├── Color.kt
    │   ├── Theme.kt
    │   └── Type.kt
    └── 📁 components/
        ├── PlayerCard.kt
        ├── GameCard.kt
        └── NumberReveal.kt
```

---

## 🔄 Flujo de Datos

### Modo Local
```
Usuario toca pantalla
        │
        ▼
    Fragment
        │ (evento)
        ▼
    ViewModel
        │ (procesa)
        ▼
    GameEngine
        │ (valida y calcula)
        ▼
    ViewModel
        │ (actualiza LiveData)
        ▼
    Fragment
        │ (observa y renderiza)
        ▼
    UI actualizada
```

### Modo LAN
```
Usuario toca pantalla
        │
        ▼
    Fragment
        │ (evento)
        ▼
    ViewModel
        │ (envía a red)
        ▼
   LANManager ──────────► Otros dispositivos
        │                       │
        │ (recibe respuesta)    │
        ◄───────────────────────┘
        │
    GameEngine
        │ (valida - solo host)
        ▼
    ViewModel
        │ (actualiza LiveData)
        ▼
    Fragment
        │ (renderiza)
        ▼
    UI sincronizada
```

---

## 🎮 Interfaces de Juego

### IGameEngine
```kotlin
interface IGameEngine<State, Action> {
    val gameState: StateFlow<State>
    
    fun startGame(players: List<Player>, mode: GameMode)
    fun processAction(action: Action): Result<State>
    fun isGameOver(): Boolean
    fun reset()
}
```

### INetworkGame (para LAN)
```kotlin
interface INetworkGame {
    fun onPlayerJoined(player: Player)
    fun onPlayerLeft(playerId: String)
    fun onActionReceived(action: GameAction)
    fun broadcastState(state: GameState)
}
```

---

## 🌐 Protocolo de Red LAN

### Tipos de Mensajes
| Tipo | Dirección | Descripción |
|------|-----------|-------------|
| `ROOM_ANNOUNCE` | Broadcast | Host anuncia sala |
| `JOIN_REQUEST` | Cliente→Host | Pedir unirse |
| `JOIN_ACCEPT` | Host→Cliente | Confirmación |
| `GAME_START` | Host→Todos | Iniciar partida |
| `ACTION` | Jugador→Host | Acción de juego |
| `STATE_UPDATE` | Host→Todos | Estado actualizado |
| `HEARTBEAT` | Bidireccional | Keep-alive |

### Formato de Mensaje (JSON)
```json
{
  "type": "ACTION",
  "gameId": "the_mind",
  "playerId": "player_1",
  "timestamp": 1707234567890,
  "data": {
    "action": "REVEAL_NUMBER",
    "value": 42
  }
}
```

---

## 📝 Decisiones de Arquitectura

### ¿Por qué MVVM?
- ✅ Recomendado oficialmente por Google
- ✅ Separa lógica de UI
- ✅ Sobrevive a cambios de configuración
- ✅ Testeable

### ¿Por qué separar GameEngine?
- ✅ Lógica de juego independiente de Android
- ✅ Fácil de testear con unit tests
- ✅ Reutilizable entre modo local y LAN

### ¿Por qué TCP para LAN?
- ✅ Garantiza entrega de mensajes
- ✅ Orden de mensajes asegurado
- ⚠️ UDP broadcast solo para descubrimiento

---

**Última actualización**: 6 de Febrero de 2026
