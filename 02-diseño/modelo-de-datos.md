# Modelo de Datos

---

## 📐 Diagrama Entidad-Relación

```
┌──────────────────┐       ┌──────────────────┐
│      Player      │       │    GameSession   │
├──────────────────┤       ├──────────────────┤
│ id: String       │◄──────│ id: String       │
│ name: String     │   *   │ gameType: GameType│
│ deviceId: String │       │ mode: GameMode   │
│ isHost: Boolean  │       │ status: Status   │
│ isConnected: Bool│       │ createdAt: Long  │
│ lives: Int       │       │ players: List    │
└──────────────────┘       └──────────────────┘
         │                          │
         │                          │
         ▼                          ▼
┌──────────────────┐       ┌──────────────────┐
│  MindGameState   │       │   AsGameState    │
├──────────────────┤       ├──────────────────┤
│ level: Int       │       │ round: Int       │
│ lives: Int       │       │ deck: List<Card> │
│ numbers: Map     │       │ hands: Map       │
│ revealed: List   │       │ currentTurn: Int │
│ isComplete: Bool │       │ eliminated: List │
└──────────────────┘       └──────────────────┘
```

---

## 📦 Clases del Modelo

### Player (Jugador)
```kotlin
data class Player(
    val id: String = UUID.randomUUID().toString(),
    val name: String,
    val deviceId: String? = null,  // null en modo local
    val isHost: Boolean = false,
    var isConnected: Boolean = true,
    var lives: Int = 3
)
```

### GameMode (Modo de Juego)
```kotlin
enum class GameMode {
    LOCAL,  // Todos en un dispositivo
    LAN     // Cada uno en su dispositivo
}
```

### GameType (Tipo de Juego)
```kotlin
enum class GameType {
    THE_MIND,
    EL_AS
    // Futuros juegos
}
```

### GameSession (Sesión de Juego)
```kotlin
data class GameSession(
    val id: String = UUID.randomUUID().toString(),
    val gameType: GameType,
    val mode: GameMode,
    val status: GameStatus,
    val createdAt: Long = System.currentTimeMillis(),
    val players: List<Player>
)

enum class GameStatus {
    WAITING,      // En sala de espera
    IN_PROGRESS,  // Partida en curso
    FINISHED      // Terminada
}
```

---

## 🔢 The Mind - Modelo de Datos

### MindGameState
```kotlin
data class MindGameState(
    val level: Int = 1,
    val lives: Int = 3,
    val playerNumbers: Map<String, List<Int>>,  // playerId -> sus números
    val revealedNumbers: List<RevealedNumber>,  // números ya revelados
    val pendingNumbers: List<Int>,               // números aún por revelar
    val status: MindStatus
)

data class RevealedNumber(
    val number: Int,
    val playerId: String,
    val wasCorrect: Boolean,
    val timestamp: Long
)

enum class MindStatus {
    SHOWING_NUMBERS,  // Mostrando números a jugadores
    PLAYING,          // Jugando
    LEVEL_COMPLETE,   // Nivel completado
    GAME_OVER,        // Sin vidas
    VICTORY           // Todos los niveles completados
}
```

### MindAction (Acciones posibles)
```kotlin
sealed class MindAction {
    data class RevealNumber(
        val playerId: String,
        val number: Int
    ) : MindAction()
    
    object NextLevel : MindAction()
    object Restart : MindAction()
}
```

---

## 🃏 El As - Modelo de Datos

### Card (Carta)
```kotlin
data class Card(
    val value: Int,  // 1-12
    val suit: Suit?  // Opcional, para visualización
) {
    val isKing: Boolean get() = value == 12
    
    companion object {
        fun createDeck(): List<Card> {
            return Suit.values().flatMap { suit ->
                (1..12).map { value -> Card(value, suit) }
            }
        }
    }
}

enum class Suit {
    OROS,
    COPAS,
    ESPADAS,
    BASTOS
}
```

### AsGameState
```kotlin
data class AsGameState(
    val round: Int = 1,
    val deck: MutableList<Card>,
    val playerHands: Map<String, Card>,   // playerId -> su carta
    val playerLives: Map<String, Int>,     // playerId -> vidas restantes
    val currentTurnIndex: Int = 0,
    val turnOrder: List<String>,           // IDs en orden de turno
    val eliminatedPlayers: List<String>,
    val lastAction: AsAction? = null,
    val status: AsStatus
)

enum class AsStatus {
    DEALING,           // Repartiendo cartas
    PLAYER_TURN,       // Turno de un jugador
    WAITING_RESPONSE,  // Esperando respuesta del otro jugador
    ROUND_END,         // Revelando cartas
    GAME_OVER          // Hay ganador
}
```

### AsAction (Acciones posibles)
```kotlin
sealed class AsAction {
    data class RequestSwap(val playerId: String) : AsAction()
    data class AcceptSwap(val playerId: String) : AsAction()
    data class DenySwap(val playerId: String, val hasKing: Boolean) : AsAction()
    data class Keep(val playerId: String) : AsAction()
    data class SwapWithDeck(val playerId: String) : AsAction()
}
```

---

## 🌐 LAN - Modelo de Datos

### Room (Sala LAN)
```kotlin
data class Room(
    val id: String = UUID.randomUUID().toString(),
    val name: String,
    val hostAddress: String,
    val hostPort: Int,
    val gameType: GameType,
    val players: List<Player>,
    val maxPlayers: Int,
    val isOpen: Boolean = true
)
```

### NetworkMessage
```kotlin
data class NetworkMessage(
    val type: MessageType,
    val gameId: String,
    val senderId: String,
    val timestamp: Long = System.currentTimeMillis(),
    val data: Map<String, Any?>
)

enum class MessageType {
    // Conexión
    ROOM_ANNOUNCE,
    JOIN_REQUEST,
    JOIN_ACCEPT,
    JOIN_REJECT,
    PLAYER_LEFT,
    
    // Juego
    GAME_START,
    ACTION,
    STATE_UPDATE,
    
    // Sistema
    HEARTBEAT,
    ERROR
}
```

---

## 💾 Persistencia Local

### Preferencias (SharedPreferences)
```kotlin
data class AppPreferences(
    val soundEnabled: Boolean = true,
    val vibrationEnabled: Boolean = true,
    val playerName: String = "Jugador",
    val lastGamePlayed: GameType? = null
)
```

### Estadísticas (Opcional)
```kotlin
data class GameStats(
    val gameType: GameType,
    val gamesPlayed: Int = 0,
    val gamesWon: Int = 0,
    val highestLevel: Int = 0  // Para The Mind
)
```

---

## 📊 Resumen de Entidades

| Entidad | Descripción | Persistencia |
|---------|-------------|--------------|
| Player | Jugador en partida | En memoria |
| GameSession | Sesión de juego | En memoria |
| MindGameState | Estado de The Mind | En memoria |
| AsGameState | Estado de El As | En memoria |
| Card | Carta de baraja | En memoria |
| Room | Sala LAN | En memoria |
| NetworkMessage | Mensaje de red | En memoria |
| AppPreferences | Preferencias | SharedPrefs |
| GameStats | Estadísticas | SharedPrefs (opcional) |

---

**Última actualización**: 6 de Febrero de 2026
