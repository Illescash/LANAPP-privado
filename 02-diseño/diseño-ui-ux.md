# Diseño UI/UX

---

## 🎨 Sistema de Diseño

### Paleta de Colores
| Nombre | Hex | Uso |
|--------|-----|-----|
| **Primary** | `#6750A4` | Botones principales, headers |
| **Primary Container** | `#EADDFF` | Fondos de cards seleccionadas |
| **Secondary** | `#625B71` | Elementos secundarios |
| **Tertiary** | `#7D5260` | Acentos especiales |
| **Surface** | `#1C1B1F` | Fondo de cards (dark) |
| **Background** | `#0E0E10` | Fondo principal (dark) |
| **Error** | `#F2B8B5` | Vidas perdidas, errores |
| **Success** | `#81C784` | Aciertos, victorias |
| **On Surface** | `#E6E1E5` | Texto sobre surfaces |

### Tipografía
| Estilo | Tamaño | Peso | Uso |
|--------|--------|------|-----|
| Display Large | 57sp | Regular | Números de juego |
| Headline Large | 32sp | Regular | Títulos de pantalla |
| Title Large | 22sp | Medium | Nombres de juegos |
| Body Large | 16sp | Regular | Texto general |
| Label Large | 14sp | Medium | Botones |

### Componentes Base
- **Buttons**: Rounded corners (28dp), min height 56dp
- **Cards**: Rounded corners (16dp), elevation 2dp
- **Iconos**: Material Symbols, peso 400
- **Espaciado**: Múltiplos de 8dp

---

## 📱 Pantallas de la Aplicación

### 1. Hub Principal

```
┌────────────────────────────────────┐
│  ⚙️                    PartyHub 🎮  │
├────────────────────────────────────┤
│                                    │
│  ┌──────────────────────────────┐  │
│  │                              │  │
│  │   🔢  THE MIND               │  │
│  │   Sincroniza mentes          │  │
│  │   2-8 jugadores              │  │
│  │                              │  │
│  └──────────────────────────────┘  │
│                                    │
│  ┌──────────────────────────────┐  │
│  │                              │  │
│  │   🃏  EL AS                   │  │
│  │   Baraja española            │  │
│  │   3-8 jugadores              │  │
│  │                              │  │
│  └──────────────────────────────┘  │
│                                    │
│  ┌──────────────────────────────┐  │
│  │   🔮  MÁS JUEGOS (Próximamente)│  │
│  └──────────────────────────────┘  │
│                                    │
└────────────────────────────────────┘
```

**Elementos**:
- Header con logo y acceso a configuración
- Cards de juegos con icono, nombre, descripción y rango de jugadores
- Scroll vertical para más juegos
- Animación hover/press en cards

---

### 2. Selección de Modo

```
┌────────────────────────────────────┐
│  ←           THE MIND              │
├────────────────────────────────────┤
│                                    │
│         ¿Cómo vais a jugar?        │
│                                    │
│  ┌──────────────────────────────┐  │
│  │                              │  │
│  │   📱  UN SOLO TELÉFONO       │  │
│  │                              │  │
│  │   Todos jugáis pasándoos     │  │
│  │   el mismo dispositivo       │  │
│  │                              │  │
│  └──────────────────────────────┘  │
│                                    │
│  ┌──────────────────────────────┐  │
│  │                              │  │
│  │   📶  CADA UNO EL SUYO       │  │
│  │                              │  │
│  │   Conectados por WiFi        │  │
│  │   (misma red)                │  │
│  │                              │  │
│  └──────────────────────────────┘  │
│                                    │
└────────────────────────────────────┘
```

---

### 3. Configuración Local

```
┌────────────────────────────────────┐
│  ←        CONFIGURAR PARTIDA       │
├────────────────────────────────────┤
│                                    │
│  Número de jugadores               │
│                                    │
│         ◀️   4 jugadores   ▶️       │
│                                    │
│  ─────────────────────────────────  │
│                                    │
│  Nombres (opcional)                │
│  ┌────────────────────────────┐    │
│  │ Jugador 1                  │    │
│  └────────────────────────────┘    │
│  ┌────────────────────────────┐    │
│  │ Jugador 2                  │    │
│  └────────────────────────────┘    │
│  ┌────────────────────────────┐    │
│  │ Jugador 3                  │    │
│  └────────────────────────────┘    │
│  ┌────────────────────────────┐    │
│  │ Jugador 4                  │    │
│  └────────────────────────────┘    │
│                                    │
│  ┌──────────────────────────────┐  │
│  │      🎮  INICIAR PARTIDA     │  │
│  └──────────────────────────────┘  │
│                                    │
└────────────────────────────────────┘
```

---

### 4. The Mind - Pantalla de Juego

```
┌────────────────────────────────────┐
│  NIVEL 3              ❤️ ❤️ ❤️      │
├────────────────────────────────────┤
│                                    │
│                                    │
│           Números revelados:       │
│                                    │
│      ┌────┐  ┌────┐  ┌────┐       │
│      │ 12 │  │ 28 │  │ 45 │       │
│      └────┘  └────┘  └────┘       │
│                                    │
│                                    │
│  ─────────────────────────────────  │
│                                    │
│      Toca cuando creas que        │
│      tienes el número más bajo    │
│                                    │
│  ┌────────────────────────────────┐│
│  │                                ││
│  │                                ││
│  │       [ÁREA TÁCTIL]            ││
│  │                                ││
│  │                                ││
│  └────────────────────────────────┘│
│                                    │
└────────────────────────────────────┘
```

**Variantes**:
- **Mostrar número secreto**: Pantalla con número grande + "Memorízalo"
- **Acierto**: Animación verde, número se añade
- **Error**: Animación roja, vida se pierde

---

### 5. El As - Pantalla de Turno

```
┌────────────────────────────────────┐
│  RONDA 2           Tu turno, Ana   │
├────────────────────────────────────┤
│                                    │
│  Cartas de los demás:              │
│                                    │
│   ┌─────┐  ┌─────┐  ┌─────┐       │
│   │  7  │  │  3  │  │ 12  │       │
│   │  🟡 │  │  🟡 │  │  👑 │       │
│   │ Bob │  │Carlos│  │Diana│       │
│   │❤️❤️❤️│  │ ❤️❤️ │  │❤️❤️❤️│       │
│   └─────┘  └─────┘  └─────┘       │
│                                    │
│  ─────────────────────────────────  │
│                                    │
│  Tu carta:  ┌─────┐                │
│             │  ?  │  (no la ves)   │
│             │ 🔙  │                │
│             └─────┘                │
│                                    │
│  ┌───────────────┐ ┌─────────────┐ │
│  │   CAMBIAR     │ │  QUEDARME   │ │
│  │  (con Carlos) │ │             │ │
│  └───────────────┘ └─────────────┘ │
│                                    │
└────────────────────────────────────┘
```

---

### 6. El As - Revelación de Ronda

```
┌────────────────────────────────────┐
│          FIN DE RONDA 2            │
├────────────────────────────────────┤
│                                    │
│         Todas las cartas:          │
│                                    │
│   ┌─────┐ ┌─────┐ ┌─────┐ ┌─────┐ │
│   │  5  │ │  3  │ │ 12  │ │  8  │ │
│   │ 🟡  │ │ 💀  │ │ 👑  │ │ 🟡  │ │
│   │ Ana │ │ Bob │ │Carlos│ │Diana│ │
│   └─────┘ └─────┘ └─────┘ └─────┘ │
│                                    │
│      💔 Bob pierde una vida        │
│          (carta más baja)          │
│                                    │
│  ─────────────────────────────────  │
│                                    │
│  ┌──────────────────────────────┐  │
│  │      SIGUIENTE RONDA         │  │
│  └──────────────────────────────┘  │
│                                    │
└────────────────────────────────────┘
```

---

### 7. Sala de Espera LAN

```
┌────────────────────────────────────┐
│  ←           SALA LAN              │
├────────────────────────────────────┤
│                                    │
│   Código: ABCD-1234                │
│   También puedes unirte por IP:    │
│   192.168.1.45:54321               │
│                                    │
│  ─────────────────────────────────  │
│                                    │
│   Jugadores (2/8):                 │
│                                    │
│   ┌────────────────────────────┐   │
│   │ 👑 Ana (Tú - Host)         │   │
│   └────────────────────────────┘   │
│   ┌────────────────────────────┐   │
│   │ 👤 Carlos                  │   │
│   └────────────────────────────┘   │
│   ┌ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ┐   │
│   │     Esperando más...       │   │
│   └ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ┘   │
│                                    │
│  ┌──────────────────────────────┐  │
│  │      🎮  INICIAR PARTIDA     │  │
│  │      (mínimo 2 jugadores)    │  │
│  └──────────────────────────────┘  │
│                                    │
└────────────────────────────────────┘
```

---

## 🎬 Flujos de Navegación

### Flujo Principal
```
    Hub
     │
     ├── The Mind ──► Selección Modo ──┬── Local ──► Config ──► Juego
     │                                 │
     │                                 └── LAN ──┬── Crear Sala ──► Espera ──► Juego
     │                                           │
     │                                           └── Unirse ──► Espera ──► Juego
     │
     └── El As ──► (mismo flujo)
```

### Flujo de Juego The Mind
```
Mostrar números → Jugar (tocar) → Revelar → ¿Correcto? 
                                              │
                         ┌────────────────────┼────────────────────┐
                         ▼                    ▼                    ▼
                    Continuar            Perder vida          ¿Todos?
                         │                    │                    │
                         │              ¿Quedan vidas?        Siguiente nivel
                         │                    │                    │
                         │            ┌───────┴───────┐      ¿Último nivel?
                         │            ▼               ▼            │
                         │         Game Over      Continuar   Victoria!
                         │                            │
                         └────────────────────────────┘
```

---

## 📁 Capturas/Mockups

> **TODO**: Añadir imágenes de mockups en esta carpeta

- `images/mockup-hub.png`
- `images/mockup-themind-game.png`
- `images/mockup-elas-turn.png`
- `images/mockup-lan-lobby.png`

---

## ✅ Checklist de Diseño

- [ ] Mockups de todas las pantallas
- [ ] Prototipo navegable (Figma/Adobe XD)
- [ ] Iconos personalizados para juegos
- [ ] Animaciones definidas
- [ ] Diseño responsivo validado
- [ ] Accesibilidad revisada

---

**Última actualización**: 6 de Febrero de 2026
