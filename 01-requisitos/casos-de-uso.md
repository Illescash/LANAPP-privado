# Casos de Uso

---

## 📋 Actores del Sistema

### 👤 Jugador Local
Usuario que juega en modo local (todos comparten el mismo dispositivo).

### 👤 Jugador Host (LAN)
Usuario que crea una sala y actúa como servidor de la partida en modo LAN.

### 👤 Jugador Cliente (LAN)
Usuario que se une a una sala existente en modo LAN.

---

## 🏠 Casos de Uso: Hub Principal

### CU-01: Seleccionar Juego
| Campo | Descripción |
|-------|-------------|
| **Actor** | Cualquier jugador |
| **Precondición** | La aplicación está abierta en la pantalla principal |
| **Flujo Principal** | 1. El usuario visualiza los juegos disponibles<br>2. El usuario toca sobre un juego<br>3. El sistema muestra la pantalla de selección de modo |
| **Postcondición** | Se muestra la pantalla de configuración del juego seleccionado |

### CU-02: Volver al Hub
| Campo | Descripción |
|-------|-------------|
| **Actor** | Cualquier jugador |
| **Precondición** | El usuario está en cualquier pantalla que no sea el hub |
| **Flujo Principal** | 1. El usuario pulsa el botón de volver/home<br>2. Si hay partida en curso, se muestra confirmación<br>3. El sistema regresa al hub principal |
| **Flujo Alternativo** | Si hay partida en curso y el usuario cancela, permanece en el juego |
| **Postcondición** | Se muestra el hub principal |

---

## 🎮 Casos de Uso: Configuración de Partida

### CU-03: Iniciar Partida Local
| Campo | Descripción |
|-------|-------------|
| **Actor** | Jugador Local |
| **Precondición** | El usuario ha seleccionado un juego |
| **Flujo Principal** | 1. El usuario selecciona modo "LOCAL"<br>2. El usuario elige número de jugadores<br>3. (Opcional) El usuario introduce nombres<br>4. El usuario pulsa "Iniciar"<br>5. El sistema inicia la partida |
| **Validaciones** | Número de jugadores dentro del rango permitido |
| **Postcondición** | La partida comienza con la configuración elegida |

### CU-04: Crear Sala LAN
| Campo | Descripción |
|-------|-------------|
| **Actor** | Jugador Host |
| **Precondición** | Dispositivo conectado a WiFi, juego seleccionado |
| **Flujo Principal** | 1. El usuario selecciona modo "LAN"<br>2. El usuario elige "Crear Sala"<br>3. El sistema genera código de sala<br>4. El sistema muestra sala de espera<br>5. Los jugadores se van uniendo<br>6. El host pulsa "Iniciar" cuando todos están listos |
| **Flujo Alternativo** | Si no hay WiFi, se muestra error y se sugiere modo local |
| **Postcondición** | La partida LAN comienza y se sincroniza |

### CU-05: Unirse a Sala LAN
| Campo | Descripción |
|-------|-------------|
| **Actor** | Jugador Cliente |
| **Precondición** | Existe una sala creada en la misma red WiFi |
| **Flujo Principal** | 1. El usuario selecciona modo "LAN"<br>2. El usuario elige "Unirse a Sala"<br>3. El sistema busca salas disponibles<br>4. El usuario selecciona una sala<br>5. El usuario introduce su nombre<br>6. El sistema conecta y muestra sala de espera |
| **Flujo Alternativo** | Si no encuentra salas, opción de introducir IP manualmente |
| **Postcondición** | El jugador está en la sala esperando inicio |

---

## 🔢 Casos de Uso: The Mind

### CU-06: Jugar Ronda (Local)
| Campo | Descripción |
|-------|-------------|
| **Actor** | Jugadores Locales |
| **Precondición** | Partida iniciada, jugadores tienen números asignados |
| **Flujo Principal** | 1. Los jugadores memorizan sus números<br>2. Se muestra pantalla de juego<br>3. Un jugador toca la pantalla cuando cree que tiene el menor<br>4. Se revela el número<br>5. Si es correcto, continúa; si no, se pierde vida<br>6. Se repite hasta revelar todos los números |
| **Flujo Alternativo** | Si se pierden todas las vidas → Game Over |
| **Postcondición** | Nivel completado o Game Over |

### CU-07: Jugar Ronda (LAN)
| Campo | Descripción |
|-------|-------------|
| **Actor** | Jugadores LAN |
| **Precondición** | Todos conectados, números asignados y sincronizados |
| **Flujo Principal** | 1. Cada jugador ve sus números en su dispositivo<br>2. Un jugador toca su pantalla cuando cree que tiene el menor<br>3. El servidor valida y notifica a todos<br>4. Se revela el número en todos los dispositivos<br>5. Se pierde vida si es incorrecto<br>6. Se repite hasta completar |
| **Postcondición** | Estado sincronizado: nivel completado o vida perdida |

### CU-08: Avanzar de Nivel
| Campo | Descripción |
|-------|-------------|
| **Actor** | Sistema |
| **Precondición** | Todos los números del nivel revelados en orden |
| **Flujo Principal** | 1. Se muestra mensaje de nivel completado<br>2. Se reparten más números (nivel+1 cartas por jugador)<br>3. Se inicia el siguiente nivel |
| **Postcondición** | Nuevo nivel iniciado con más dificultad |

---

## 🃏 Casos de Uso: El As

### CU-09: Jugar Turno (Local)
| Campo | Descripción |
|-------|-------------|
| **Actor** | Jugador activo |
| **Precondición** | Es su turno, tiene carta asignada |
| **Flujo Principal** | 1. Se pasa el teléfono al jugador activo<br>2. El jugador ve las cartas de los demás (no la suya)<br>3. El jugador decide: CAMBIAR o QUEDARSE<br>4. Si cambia: el de la derecha acepta o muestra 12<br>5. Se pasa al siguiente jugador |
| **Flujo Alternativo** | Si es el último y cambia → intercambia con el mazo |
| **Postcondición** | Turno completado, pasa al siguiente |

### CU-10: Jugar Turno (LAN)
| Campo | Descripción |
|-------|-------------|
| **Actor** | Jugador activo (en su dispositivo) |
| **Precondición** | Es su turno según el servidor |
| **Flujo Principal** | 1. El jugador recibe notificación de su turno<br>2. Ve en su pantalla las cartas de los demás<br>3. Decide CAMBIAR o QUEDARSE<br>4. Envía acción al servidor<br>5. Servidor procesa y notifica a todos |
| **Postcondición** | Acción sincronizada en todos los dispositivos |

### CU-11: Resolver Ronda
| Campo | Descripción |
|-------|-------------|
| **Actor** | Sistema |
| **Precondición** | Todos los jugadores han jugado su turno |
| **Flujo Principal** | 1. Se revelan todas las cartas<br>2. Se identifica la carta más baja<br>3. Ese jugador pierde 1 vida<br>4. Se verifica si tiene vidas restantes<br>5. Se prepara la siguiente ronda |
| **Flujo Alternativo** | Si empate → todos los empatados pierden vida |
| **Postcondición** | Vidas actualizadas, nueva ronda o fin de partida |

### CU-12: Eliminar Jugador
| Campo | Descripción |
|-------|-------------|
| **Actor** | Sistema |
| **Precondición** | Un jugador llega a 0 vidas |
| **Flujo Principal** | 1. Se muestra animación de eliminación<br>2. El jugador es marcado como eliminado<br>3. Se verifica si queda más de 1 jugador<br>4. Si sí, continúa la partida<br>5. Si no, se declara ganador |
| **Postcondición** | Jugador eliminado o partida finalizada |

---

## 📊 Diagrama de Casos de Uso

```
                    ┌─────────────────────────────────────────┐
                    │              PartyHub                    │
                    └─────────────────────────────────────────┘
                                      │
            ┌─────────────────────────┼─────────────────────────┐
            │                         │                         │
    ┌───────▼───────┐         ┌───────▼───────┐         ┌───────▼───────┐
    │  Hub Principal │         │   The Mind    │         │    El As      │
    └───────────────┘         └───────────────┘         └───────────────┘
    │ - Seleccionar  │         │ - Jugar Ronda │         │ - Jugar Turno │
    │   juego        │         │ - Avanzar     │         │ - Cambiar     │
    │ - Volver       │         │   nivel       │         │   carta       │
    └────────────────┘         │ - Game Over   │         │ - Resolver    │
                               └───────────────┘         │   ronda       │
            ┌─────────────────────────┐                  │ - Eliminar    │
            │   Configuración         │                  └───────────────┘
            └─────────────────────────┘
            │ - Iniciar Local         │
            │ - Crear Sala            │
            │ - Unirse a Sala         │
            └─────────────────────────┘

                    👤                          👤
              Jugador Local              Jugador LAN (Host/Cliente)
```

---

## 📝 Notas

- Los casos de uso LAN dependen de la conectividad WiFi
- El actor "Sistema" representa la lógica interna de la aplicación
- Cada caso de uso debe tener tests asociados en la fase de verificación

---

**Última actualización**: 6 de Febrero de 2026
