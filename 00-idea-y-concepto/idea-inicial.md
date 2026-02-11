# Idea Inicial de la Aplicación

---

## 🎯 Nombre de la Aplicación

**PartyHub** *(nombre provisional - alternativas: GameNight, PlayTogether, LocalParty)*

---

## 💡 Concepto General

### ¿Qué es?
PartyHub es un **hub de minijuegos** diseñado para jugar con amigos de manera **presencial**. La aplicación ofrece una colección de juegos clásicos y divertidos que se pueden jugar en reuniones, fiestas o quedadas.

### ¿Qué problema resuelve?
- **Falta de entretenimiento grupal móvil**: La mayoría de juegos móviles son individuales o requieren internet
- **Barrera de entrada**: No todos tienen el mismo juego instalado o quieren descargar apps
- **Flexibilidad de modos**: Permite jugar en un solo dispositivo (local) o cada uno con el suyo (LAN)
- **Alternativa digital a juegos físicos**: Cartas, dados, números... sin necesidad de materiales

---

## 👥 Usuario Objetivo

### Perfil del usuario
- **Edad**: 16-35 años principalmente
- **Características**: Personas sociables, que disfrutan de reuniones con amigos
- **Necesidades**: Entretenimiento rápido y divertido sin complicaciones
- **Contexto de uso**: Fiestas, quedadas, viajes, tiempos de espera en grupo

### Escenarios de uso
1. **Fiesta en casa**: Un grupo de amigos quiere jugar a algo rápido mientras charlan
2. **Viaje en grupo**: Durante un descanso en un viaje, buscan entretenimiento sin internet
3. **Reunión familiar**: Juegos intergeneracionales que todos pueden entender
4. **Pre-game/After**: Mientras esperan entre planes, un juego rápido anima el ambiente

---

## ⚡ Propuesta de Valor

### ¿Por qué usar esta app?
- 🎮 **Variedad de juegos** en una sola app
- 📱 **Modo Local**: Un solo teléfono para todos - sin necesidad de que todos instalen
- 🌐 **Modo LAN**: Cada jugador en su dispositivo, conectados sin internet
- ⚡ **Partidas rápidas**: Diseñados para diversión inmediata
- 🎯 **Juegos probados**: Basados en clásicos que ya son divertidos

### Diferenciadores
1. **Dualidad Local/LAN** en cada juego - única en el mercado
2. **No requiere internet** - todo funciona offline
3. **Juegos clásicos adaptados** - curva de aprendizaje mínima
4. **Diseño para interacción presencial** - fomenta la socialización real

---

## 🎮 Juegos Incluidos

### 🔢 Juego 1: "The Mind" / "La Mente"

**Concepto**: Juego cooperativo de sincronización mental sin comunicación verbal.

**Reglas**:
1. Cada jugador recibe un número del 1 al 100 en secreto
2. Sin hablar ni comunicarse, deben ordenarse de menor a mayor
3. **Modo Local**: Los jugadores van colocando la mano sobre la mesa/teléfono uno encima de otro
4. **Modo LAN**: Los jugadores van tocando su pantalla cuando creen que es su turno
5. Si alguien toca cuando no le corresponde (hay números menores sin revelar), pierden una vida
6. El objetivo es completar todas las rondas (se añaden más cartas por ronda)

**Atractivo**: Genera tensión, risas y momentos memorables. Funciona sorprendentemente bien.

---

### 🃏 Juego 2: "El As" / "Culo"

**Concepto**: Juego clásico de baraja española de eliminación por rondas.

**Reglas**:
1. Cada jugador recibe una carta (1-12, donde 1 es peor y 12 es mejor)
2. **Sin ver su propia carta**, cada turno decide si cambiarla con el jugador a su derecha
3. El jugador de la derecha **debe aceptar** el cambio, a menos que tenga un 12 (Rey) → se niega
4. Los turnos rotan hacia la derecha
5. El último jugador puede cambiar con el mazo (sin excepción)
6. Al final de la ronda, quien tenga la carta más baja pierde **1 vida**
7. Cada jugador tiene **3 vidas** - quien las pierde queda eliminado
8. Gana el último en pie

**Modalidades**:
- **Local**: Todos juegan en un dispositivo, pasándoselo
- **LAN**: Cada jugador ve su carta en su pantalla, las acciones se sincronizan

---

### 🔮 Juegos Futuros (Ideas)
- **Ruleta de retos**: Gira la ruleta y haz el reto
- **Piedra, papel, tijera torneo**: Eliminatoria rápida
- **Dados mentiroso**: Clásico juego de faroleo con dados
- **Quién es más probable**: Votar quién del grupo haría X cosa

---

## 🎨 Visión General

### Funcionalidades Principales
1. **Hub central**: Menú con todos los juegos disponibles
2. **Configuración de partida**: Elegir modo (Local/LAN), número de jugadores
3. **Sistema de salas LAN**: Crear/unirse a partidas en la misma red WiFi
4. **Juegos completos**: Lógica, UI y feedback de cada juego
5. **Historial/Estadísticas**: Quién ha ganado más (opcional)

### Flujo Principal del Usuario
```
[Abrir app] → [Seleccionar juego] → [Elegir modo: Local/LAN] 
     ↓                                      ↓
[Configurar partida]              [Crear/Unirse a sala]
     ↓                                      ↓
          [Jugar] → [Resultado] → [Volver al hub]
```

---

## 🔍 Análisis Inicial

### Viabilidad Técnica
- ✅ **Juegos sencillos**: Lógica fácil de implementar
- ✅ **Modo local**: Trivial, un solo dispositivo
- ⚠️ **Modo LAN**: Requiere implementar comunicación por sockets (desafío técnico interesante)
- ✅ **Sin backend**: Todo funciona offline/local
- ✅ **UI simple pero atractiva**: Cartas, números, botones grandes

### Desafíos Técnicos
1. **Descubrimiento de dispositivos en LAN** (UDP broadcast o similar)
2. **Sincronización en tiempo real** entre dispositivos
3. **Manejo de desconexiones** durante partida LAN
4. **Diseño de UI que sirva para local y LAN** con adaptaciones

### Alcance del Proyecto

**MVP (Entrega 2)**:
- Hub con 2 juegos
- Ambos juegos funcionando en modo LOCAL
- UI completa y funcional

**Versión Final (Entrega 3)**:
- Modo LAN implementado en ambos juegos
- Sistema de salas (crear/unirse)
- Pulido visual y sonidos
- Posibles juegos adicionales si hay tiempo

---

## 📊 Estado Actual

- [x] Idea definida
- [x] Usuario objetivo identificado
- [x] Funcionalidades principales listadas
- [ ] Referencias investigadas
- [x] Viabilidad técnica evaluada
- [ ] Diseño UI/UX iniciado

---

## 💭 Decisiones Pendientes

1. **Nombre definitivo de la app**: PartyHub, GameNight, LocalParty...?
2. **Nombres de los juegos**: ¿Usar nombres conocidos o inventar propios?
3. **Identidad visual**: Colores, estilo (moderno, retro, minimalista...)
4. **¿Incluir más juegos inicialmente?**: Valorar según tiempo disponible

---

**Fecha de creación**: 6 de Febrero de 2026
**Última actualización**: 6 de Febrero de 2026
