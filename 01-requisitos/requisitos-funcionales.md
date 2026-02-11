# Requisitos Funcionales

---

## 📋 Definición

Los requisitos funcionales describen **qué debe hacer** la aplicación PartyHub. Cada requisito representa una funcionalidad específica que el sistema debe proporcionar.

---

## 🏠 Módulo: Hub Principal

### RF-01: Pantalla Principal del Hub
- **Prioridad**: Alta
- **Descripción**: La aplicación debe mostrar un menú principal con todos los juegos disponibles en forma de tarjetas o iconos seleccionables
- **Criterios de aceptación**:
  - [ ] Se muestran todos los juegos disponibles (The Mind, El As)
  - [ ] Cada juego tiene icono, nombre y breve descripción
  - [ ] Se puede acceder a cada juego con un solo tap
  - [ ] El diseño es atractivo y coherente
- **Notas**: Diseño escalable para añadir más juegos en el futuro

### RF-02: Navegación entre Pantallas
- **Prioridad**: Alta
- **Descripción**: El usuario debe poder navegar fácilmente entre el hub, la configuración de partida y los juegos
- **Criterios de aceptación**:
  - [ ] Botón de volver visible en todas las pantallas
  - [ ] Transiciones fluidas entre pantallas
  - [ ] Estado consistente al volver atrás
- **Notas**: Usar Navigation Component de Android

---

## 🎮 Módulo: Configuración de Partida

### RF-03: Selección de Modo de Juego
- **Prioridad**: Alta
- **Descripción**: Al seleccionar un juego, el usuario debe elegir entre modo Local o modo LAN
- **Criterios de aceptación**:
  - [ ] Se muestran dos opciones claramente diferenciadas: LOCAL y LAN
  - [ ] Cada opción tiene descripción breve (ej: "Un solo teléfono" / "Cada uno en el suyo")
  - [ ] La selección lleva a la configuración correspondiente
- **Notas**: El modo LAN puede estar deshabilitado si no hay WiFi

### RF-04: Configuración Modo Local
- **Prioridad**: Alta
- **Descripción**: Configurar partida para jugar todos en un mismo dispositivo
- **Criterios de aceptación**:
  - [ ] Selector de número de jugadores (2-8 según juego)
  - [ ] Opción de introducir nombres de jugadores (opcional)
  - [ ] Botón de iniciar partida
  - [ ] Resumen de configuración visible
- **Notas**: Validar rango de jugadores según juego

### RF-05: Configuración Modo LAN - Crear Sala
- **Prioridad**: Media
- **Descripción**: Un jugador puede crear una sala/partida a la que otros se unirán
- **Criterios de aceptación**:
  - [ ] Se genera un código/nombre de sala visible
  - [ ] Se muestra la IP del host para referencia
  - [ ] Lista de jugadores conectados en tiempo real
  - [ ] Indicador de "esperando jugadores"
  - [ ] Botón de iniciar partida (solo host) cuando hay suficientes jugadores
- **Notas**: Requiere implementación de servidor UDP/TCP

### RF-06: Configuración Modo LAN - Unirse a Sala
- **Prioridad**: Media
- **Descripción**: Jugadores pueden buscar y unirse a salas existentes en la misma red
- **Criterios de aceptación**:
  - [ ] Lista de salas disponibles en la red local
  - [ ] Opción de refrescar lista
  - [ ] Opción de introducir IP manualmente
  - [ ] Introducir nombre de jugador antes de unirse
  - [ ] Confirmación visual al unirse exitosamente
  - [ ] Sala de espera hasta que el host inicie
- **Notas**: Usar UDP broadcast para descubrimiento

---

## 🔢 Módulo: Juego "The Mind"

### RF-07: Reparto de Números
- **Prioridad**: Alta
- **Descripción**: Al iniciar ronda, cada jugador recibe un número único del 1 al 100
- **Criterios de aceptación**:
  - [ ] Los números son aleatorios y no repetidos dentro de la ronda
  - [ ] Cada jugador ve SOLO su número en secreto
  - [ ] La cantidad de números por jugador aumenta con cada nivel
  - [ ] Los números se reparten de forma segura (no visibles para otros)
- **Notas**: Nivel 1 = 1 carta, Nivel 2 = 2 cartas, etc.

### RF-08: Mecánica de Juego - Modo Local
- **Prioridad**: Alta
- **Descripción**: Los jugadores tocan la pantalla cuando creen que es su turno
- **Criterios de aceptación**:
  - [ ] Pantalla de juego muestra instrucción clara ("Toca cuando creas que tienes el número más bajo")
  - [ ] Al tocar, se revela el número del jugador
  - [ ] Se valida si el número revelado es correcto (el más bajo de los pendientes)
  - [ ] Si es incorrecto: feedback de error, se pierde una vida
  - [ ] Si es correcto: feedback positivo, continúa la ronda
  - [ ] Se completa el nivel cuando todos los números están revelados en orden
- **Notas**: Considerar animaciones para revelar números

### RF-09: Mecánica de Juego - Modo LAN
- **Prioridad**: Media
- **Descripción**: Cada jugador ve su número en su dispositivo y toca cuando cree que es su turno
- **Criterios de aceptación**:
  - [ ] Cada dispositivo muestra el/los número(s) del jugador
  - [ ] Al tocar, se sincroniza con todos los dispositivos
  - [ ] Todos ven qué número se ha revelado
  - [ ] El orden de toques determina si es correcto o no
  - [ ] Feedback sincronizado (vida perdida, nivel completado)
- **Notas**: Latencia crítica - necesita sincronización rápida

### RF-10: Sistema de Vidas y Niveles
- **Prioridad**: Alta
- **Descripción**: Gestión de vidas del grupo y progresión de niveles
- **Criterios de aceptación**:
  - [ ] El grupo comienza con X vidas (configurable, default 3)
  - [ ] Perder una vida se muestra claramente
  - [ ] Al perder todas las vidas: Game Over con pantalla de resultado
  - [ ] Al completar nivel: transición al siguiente con más cartas
  - [ ] Pantalla de victoria al completar todos los niveles (ej: nivel 8)
- **Notas**: Considerar power-ups como "shuriken" del juego original (opcional)

---

## 🃏 Módulo: Juego "El As"

### RF-11: Reparto de Cartas
- **Prioridad**: Alta
- **Descripción**: Cada jugador recibe una carta de la baraja española (1-12)
- **Criterios de aceptación**:
  - [ ] Las cartas van del 1 (As, la peor) al 12 (Rey, la mejor)
  - [ ] Se reparte una carta aleatoria a cada jugador
  - [ ] **El jugador NO VE su propia carta** (carta boca abajo para él)
  - [ ] Las cartas de otros jugadores SÍ son visibles (lógica del juego)
  - [ ] El mazo contiene el resto de cartas para intercambio final
- **Notas**: Implementar lógica de baraja con cartas únicas

### RF-12: Sistema de Turnos
- **Prioridad**: Alta
- **Descripción**: Los turnos rotan en orden (hacia la derecha)
- **Criterios de aceptación**:
  - [ ] Se indica claramente de quién es el turno actual
  - [ ] El turno inicial rota cada ronda (el primero de la ronda anterior va último)
  - [ ] El jugador activo tiene tiempo para decidir
  - [ ] Indicador visual del orden de turnos
- **Notas**: Modo local = pasar el teléfono; Modo LAN = cada uno en su pantalla

### RF-13: Mecánica de Intercambio
- **Prioridad**: Alta
- **Descripción**: El jugador decide si cambiar su carta con el de su derecha
- **Criterios de aceptación**:
  - [ ] Opción clara: "CAMBIAR" o "QUEDARSE"
  - [ ] Si elige cambiar: el de la derecha DEBE aceptar, EXCEPTO si tiene un 12
  - [ ] Si el de la derecha tiene 12: se niega automáticamente (con animación/sfx)
  - [ ] El intercambio es visible para todos
  - [ ] El último jugador puede cambiar con el mazo (siempre)
- **Notas**: El intercambio con el mazo revela la carta del mazo a todos

### RF-14: Sistema de Vidas y Eliminación
- **Prioridad**: Alta
- **Descripción**: Cada jugador tiene 3 vidas, pierde quien tenga la carta más baja
- **Criterios de aceptación**:
  - [ ] Cada jugador comienza con 3 vidas (corazones, monedas...)
  - [ ] Al final de la ronda: se revelan todas las cartas
  - [ ] Quien tenga la carta más baja pierde 1 vida
  - [ ] En caso de empate: todos los empatados pierden vida
  - [ ] Jugador sin vidas = eliminado de la partida
  - [ ] Partida termina cuando queda 1 jugador (o 0 si empatan los últimos)
- **Notas**: Animación épica para revelar el perdedor

### RF-15: Interfaz de Juego - Modo Local
- **Prioridad**: Alta
- **Descripción**: UI para jugar todos en el mismo dispositivo
- **Criterios de aceptación**:
  - [ ] Pantalla de "Pasa el teléfono a [Jugador X]"
  - [ ] El jugador ve las cartas de los demás pero NO la suya
  - [ ] Botones grandes y claros para CAMBIAR / QUEDARSE
  - [ ] Confirmación antes de pasar al siguiente
  - [ ] Pantalla de revelación al final de la ronda
- **Notas**: Evitar que el jugador vea su carta accidentalmente

### RF-16: Interfaz de Juego - Modo LAN
- **Prioridad**: Media
- **Descripción**: UI sincronizada entre dispositivos
- **Criterios de aceptación**:
  - [ ] Cada jugador ve su pantalla con las cartas de los demás
  - [ ] Solo el jugador activo puede interactuar
  - [ ] Las acciones se reflejan en todos los dispositivos
  - [ ] Estado de la partida sincronizado (vidas, cartas, turno)
  - [ ] Notificación cuando es tu turno
- **Notas**: Considerar vibración cuando sea tu turno

---

## 🌐 Módulo: Conectividad LAN

### RF-17: Descubrimiento de Dispositivos
- **Prioridad**: Media
- **Descripción**: Encontrar automáticamente otros dispositivos en la misma red WiFi
- **Criterios de aceptación**:
  - [ ] Broadcast UDP para anunciar/descubrir salas
  - [ ] Listado de salas encontradas con nombre y jugadores
  - [ ] Actualización periódica o manual de la lista
  - [ ] Timeout para salas inactivas
- **Notas**: Usar puerto específico (ej: 54321)

### RF-18: Conexión P2P o Cliente-Servidor
- **Prioridad**: Media
- **Descripción**: Establecer conexión estable entre dispositivos
- **Criterios de aceptación**:
  - [ ] Host actúa como servidor de la partida
  - [ ] Clientes se conectan al host vía TCP
  - [ ] Protocolo de mensajes definido (JSON o Protobuf)
  - [ ] Reconexión automática si se pierde conexión temporal
- **Notas**: Decidir arquitectura: TCP vs UDP vs WebSocket

### RF-19: Sincronización de Estado
- **Prioridad**: Media
- **Descripción**: Mantener el estado del juego consistente entre todos los dispositivos
- **Criterios de aceptación**:
  - [ ] El host es la fuente de verdad del estado
  - [ ] Los clientes envían acciones, el host las valida y distribuye
  - [ ] Latencia máxima aceptable: 500ms
  - [ ] Manejo de conflictos si dos acciones llegan simultáneas
- **Notas**: Estado incluye: jugadores, cartas/números, vidas, turno

### RF-20: Manejo de Desconexiones
- **Prioridad**: Baja
- **Descripción**: Gestionar cuando un jugador pierde conexión
- **Criterios de aceptación**:
  - [ ] Detectar desconexión en <5 segundos
  - [ ] Notificar a todos los jugadores
  - [ ] Opciones: esperar reconexión, expulsar, pausar partida
  - [ ] Si host se desconecta: migrar host o terminar partida
- **Notas**: Implementar heartbeat cada 2 segundos

---

## ⚙️ Módulo: Configuración General

### RF-21: Ajustes de la Aplicación
- **Prioridad**: Baja
- **Descripción**: Pantalla de configuración general
- **Criterios de aceptación**:
  - [ ] Activar/desactivar sonidos
  - [ ] Activar/desactivar vibración
  - [ ] Tema claro/oscuro (opcional)
  - [ ] Información de la app (versión, créditos)
- **Notas**: Persistir preferencias en SharedPreferences

### RF-22: Tutorial/Ayuda
- **Prioridad**: Baja
- **Descripción**: Explicación de reglas de cada juego
- **Criterios de aceptación**:
  - [ ] Accesible desde el hub o dentro del juego
  - [ ] Explicación clara y visual de las reglas
  - [ ] Opción de "saltarse" para jugadores experimentados
- **Notas**: Puede ser un carrusel de imágenes o texto formateado

---

## 📊 Resumen de Requisitos

| ID | Nombre | Módulo | Prioridad | Estado | Entrega |
|----|--------|--------|-----------|--------|---------|
| RF-01 | Hub Principal | Hub | Alta | ⏳ | E2 |
| RF-02 | Navegación | Hub | Alta | ⏳ | E2 |
| RF-03 | Selección Modo | Config | Alta | ⏳ | E2 |
| RF-04 | Config Local | Config | Alta | ⏳ | E2 |
| RF-05 | Crear Sala LAN | Config | Media | ⏳ | E3 |
| RF-06 | Unirse Sala LAN | Config | Media | ⏳ | E3 |
| RF-07 | Reparto Números (Mind) | The Mind | Alta | ⏳ | E2 |
| RF-08 | Juego Local (Mind) | The Mind | Alta | ⏳ | E2 |
| RF-09 | Juego LAN (Mind) | The Mind | Media | ⏳ | E3 |
| RF-10 | Vidas/Niveles (Mind) | The Mind | Alta | ⏳ | E2 |
| RF-11 | Reparto Cartas (As) | El As | Alta | ⏳ | E2 |
| RF-12 | Sistema Turnos (As) | El As | Alta | ⏳ | E2 |
| RF-13 | Intercambio (As) | El As | Alta | ⏳ | E2 |
| RF-14 | Vidas/Eliminación (As) | El As | Alta | ⏳ | E2 |
| RF-15 | UI Local (As) | El As | Alta | ⏳ | E2 |
| RF-16 | UI LAN (As) | El As | Media | ⏳ | E3 |
| RF-17 | Descubrimiento LAN | LAN | Media | ⏳ | E3 |
| RF-18 | Conexión P2P | LAN | Media | ⏳ | E3 |
| RF-19 | Sincronización | LAN | Media | ⏳ | E3 |
| RF-20 | Desconexiones | LAN | Baja | ⏳ | E3 |
| RF-21 | Ajustes | Config | Baja | ⏳ | E3 |
| RF-22 | Tutorial | Config | Baja | ⏳ | E3 |

**Estados**: ⏳ Pendiente | 🚧 En desarrollo | ✅ Implementado | ✔️ Verificado

**Entregas**: E2 = Entrega 2 (30%) | E3 = Entrega 3 (70%)

---

## 📝 Notas de Planificación

### Entrega 2 - MVP (Modo Local)
- Hub funcional con 2 juegos
- The Mind completo en modo local
- El As completo en modo local
- **Total: 12 requisitos de prioridad Alta**

### Entrega 3 - Versión Completa (Modo LAN)
- Añadir conectividad LAN a ambos juegos
- Sistema de salas
- Sincronización en tiempo real
- Ajustes y pulido
- **Total: 10 requisitos adicionales**

---

**Última actualización**: 6 de Febrero de 2026
