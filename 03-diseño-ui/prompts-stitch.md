# Prompts para Stitch - Diseño de Vistas PartyHub

---

## 📱 Paleta de Colores y Estilo

### Colores Base
- **Primary**: `#6C63FF` (Morado vibrante)
- **Secondary**: `#FF6584` (Rosa coral)
- **Accent**: `#FFD93D` (Amarillo dorado)
- **Background**: `#1A1A2E` (Azul oscuro profundo)
- **Surface**: `#16213E` (Azul grisáceo oscuro)
- **Error**: `#FF4757`
- **Success**: `#00D9A3`
- **Text Primary**: `#FFFFFF`
- **Text Secondary**: `#A0A0B8`

### Estilo Visual
- **Tipografía**: Sans-serif moderna, bold para títulos
- **Bordes**: Redondeados (16-24dp corner radius)
- **Sombras**: Elevación suave para dar profundidad
- **Animaciones**: Transiciones fluidas, micro-interacciones
- **Iconografía**: Iconos filled estilo Material Design 3

---

## 1️⃣ Vista: Hub Principal (HubView)

### Prompt para Stitch:

```
Diseña una pantalla de hub principal para una app de juegos móvil llamada "PartyHub". 

DISEÑO:
- Orientación: Vertical (portrait)
- Header superior con:
  - Logo/título "PartyHub" centrado
  - Icono de ajustes (engranaje) en esquina superior derecha
  - Altura: ~72dp
  
CONTENIDO PRINCIPAL:
- Grid de 2 columnas con tarjetas de juegos
- Cada tarjeta debe tener:
  - Icono representativo del juego (grande, ilustrativo)
  - Nombre del juego
  - Descripción breve (1 línea)
  - Indicador de jugadores (ej: "2-8 jugadores")
  - Efecto de elevación y borde redondeado
  - Estado hover/pressed visible
  
JUEGOS A MOSTRAR:
1. "The Mind" - Juego de sincronización mental con números
   - Icono: Cerebro/mente con números flotantes
   - Color dominante: Morado (#6C63FF)
   
2. "El As" - Juego de cartas de eliminación
   - Icono: Carta de As o baraja española
   - Color dominante: Rosa coral (#FF6584)
   
FOOTER:
- Botón destacado "Historial" o "Estadísticas" (opcional)

ESTILO:
- Fondo gradiente de #1A1A2E a #16213E
- Tarjetas con glassmorphism sutil
- Texto blanco con sombras para legibilidad
- Espaciado generoso (16-24dp)
- Animaciones suaves al tocar tarjetas

DIMENSIONES DE REFERENCIA:
- Pantalla: 360x800dp (teléfono estándar)
```

---

## 2️⃣ Vista: Configuración de Partida (SetupView)

### Prompt para Stitch:

```
Diseña una pantalla de configuración de partida para una app de juegos móvil.

HEADER:
- Botón atrás (flecha izquierda) en esquina superior izquierda
- Título del juego seleccionado centrado
- Altura: ~56dp

CONTENIDO PRINCIPAL:

1. SECCIÓN "Modo de Juego":
   - Dos botones grandes tipo toggle (radio buttons grandes):
     · "LOCAL" - Mismo dispositivo, icono de teléfono único
     · "LAN" - Red WiFi, icono de varios dispositivos conectados
   - Botón seleccionado con color primario (#6C63FF)
   - Botón no seleccionado con borde outline

2. SECCIÓN "Jugadores":
   - Si modo LOCAL:
     · Selector numérico (spinner/stepper) de 2 a 8 jugadores
     · Botones +/- grandes
   - Si modo LAN:
     · Mostrar "Esperando jugadores..." con spinner
     · Lista de jugadores conectados (avatares + nombres)

3. SECCIÓN "Opciones":
   - Input de texto: "Tu nombre" (para identificarse)
   - Switch: "Sonido" (ON/OFF)
   - Switch: "Vibración" (ON/OFF)

FOOTER:
- Botón CTA grande: "COMENZAR PARTIDA" (modo local) o "CREAR SALA" (modo LAN)
- Botón secundario outline: "UNIRSE A SALA" (solo en modo LAN)

ESTILO:
- Fondo: #1A1A2E
- Secciones separadas con cards (#16213E)
- Inputs con bordes redondeados
- Botón principal: degradado de #6C63FF a #5548E8
- Espaciado vertical: 16dp entre secciones
- Padding lateral: 20dp

DIMENSIONES: 360x800dp
```

---

## 3️⃣ Vista: Sala LAN (LANLobbyView)

### Prompt para Stitch:

```
Diseña una pantalla de sala de espera LAN para un juego multijugador móvil.

HEADER:
- Botón "Salir" (X) en esquina superior izquierda
- Título: "Sala de [Nombre del juego]"
- Código de sala (6 dígitos) debajo del título

CONTENIDO PRINCIPAL:

1. INFORMACIÓN DE LA SALA:
   - Nombre de la sala
   - Juego seleccionado
   - Host identificado con corona/badge

2. LISTA DE JUGADORES:
   - Grid vertical de jugadores conectados
   - Cada jugador:
     · Avatar circular (color único por jugador)
     · Nombre del jugador
     · Badge "HOST" si corresponde
     · Indicador de conexión (punto verde)
   - Jugadores esperando (slots vacíos):
     · Placeholder gris con icono de usuario
     · Texto "Esperando..."
   - Máximo 8 jugadores visibles

3. INDICADOR DE ESTADO:
   - Si eres host: "Esperando jugadores..."
   - Si eres cliente: "Esperando que el host inicie..."
   - Animación de puntos suspensivos

FOOTER:
- Si eres HOST:
  · Botón primario: "INICIAR PARTIDA" 
  · Habilitado solo si hay mínimo 2 jugadores
- Si eres CLIENTE:
  · Texto: "El host iniciará la partida"
  
ACCIONES ADICIONALES:
- Botón "Copiar código" junto al código de sala

ESTILO:
- Fondo: gradiente #1A1A2E
- Card central con glassmorphism
- Avatares: gradientes circulares únicos
- Código de sala: monospace, grande, seleccionable
- Indicadores de conexión: punto verde pulsante
- Sombras suaves en elementos flotantes

DIMENSIONES: 360x800dp
```

---

## 4️⃣ Vista: The Mind - Pantalla de Números (MindNumbersView)

### Prompt para Stitch:

```
Diseña la pantalla de juego para "The Mind", un juego de sincronización mental.

HEADER:
- Botón menú/pausa (tres puntos) esquina superior derecha
- Información de partida:
  · "Nivel [X]/10" a la izquierda
  · Iconos de vidas restantes (corazones) a la derecha
  · Barra de progreso sutil debajo

ZONA PRINCIPAL - ÁREA DE NÚMEROS REVELADOS:
- Timeline vertical central que muestra números ya jugados
- Los últimos 3-5 números revelados, ordenados de menor a mayor
- Cada número:
  · Círculo grande con el número dentro
  · Color indicador: verde si fue correcto, rojo si fue error
  · Nombre del jugador que lo jugó (pequeño, debajo)
- Animación smooth al añadir nuevo número

ZONA CENTRAL - TU NÚMERO:
- Card destacada en centro/parte inferior
- Si modo LOCAL:
  · Texto: "Presiona cuando creas que es tu turno"
  · El número está oculto hasta que el jugador toca
- Si modo LAN:
  · Muestra TU número (grande, destacado)
  · Texto: "Presiona cuando creas que es el menor"
  
BOTÓN DE ACCIÓN:
- Botón circular grande (FAB) en parte inferior central
- Icono: mano tocando / check
- Color: gradiente morado (#6C63FF)
- Efecto ripple al presionar
- Feedback háptico

INDICADORES:
- Si modo LAN: pequeños avatares de otros jugadores arriba, mostrando quién está "pensando"

ESTILO:
- Fondo: degradado oscuro (#1A1A2E)
- Números revelados: glassmorphism con glow sutil
- Tu número: card con elevación alta, borde brillante
- Animaciones: números vuelan desde botón hacia timeline
- Feedback visual inmediato en errores (shake, color rojo)

DIMENSIONES: 360x800dp
```

---

## 5️⃣ Vista: The Mind - Resultado de Nivel (MindResultView)

### Prompt para Stitch:

```
Diseña una pantalla de resultado tras completar un nivel en "The Mind".

CONTENIDO:

1. RESULTADO PRINCIPAL:
   - Si ÉXITO:
     · Icono grande: check/estrella animado
     · Texto: "¡Nivel [X] Completado!"
     · Color: verde (#00D9A3) con efecto glow
     · Confetti o animación celebratoria
   
   - Si FALLO:
     · Icono: corazón roto
     · Texto: "Nivel Fallido"
     · Color: rojo (#FF4757)
     · Vidas perdidas destacadas

2. RESUMEN DE RONDA:
   - Timeline de todos los números jugados en orden
   - Indicador visual de dónde se cometió el error (si aplica)
   - Nombres de jugadores debajo de cada número

3. ESTADÍSTICAS:
   - Vidas restantes (corazones)
   - Niveles completados vs totales
   - Tiempo transcurrido (opcional)

FOOTER:
- Si hay vidas:
  · Botón primario: "SIGUIENTE NIVEL"
- Si no hay vidas:
  · Texto: "Fin del Juego"
  · Botón: "JUGAR DE NUEVO"
  · Botón secundario: "VOLVER AL HUB"

ESTILO:
- Fondo: gradiente oscuro
- Card central con resultado
- Animaciones de entrada (scale in)
- Colores según resultado (verde/rojo)
- Transiciones suaves

DIMENSIONES: 360x800dp
```

---

## 6️⃣ Vista: El As - Pantalla de Juego (ElAsGameView)

### Prompt para Stitch:

```
Diseña la pantalla de juego para "El As", un juego de cartas de baraja española.

HEADER:
- Botón menú (tres puntos) esquina superior derecha
- Info de ronda: "Ronda [X]"
- Vidas de todos los jugadores (iconos pequeños)

CONTENIDO:

1. ÁREA DE JUGADORES:
   - Disposición circular/radial de avatares de jugadores
   - Cada jugador:
     · Avatar circular
     · Nombre
     · Vidas restantes (corazones)
     · Indicador de turno (borde brillante si es su turno)
     · Estado de carta (boca abajo hasta final de ronda)

2. TU CARTA (CENTRO INFERIOR):
   - Si modo LOCAL: carta boca ABAJO con botón "VER MI CARTA"
   - Si modo LAN: carta boca ARRIBA grande (ves tu carta)
   - Diseño de carta estilo baraja española
   - Valores: 1 (As) a 12 (Rey)
   - Palos opcionales: oros, copas, espadas, bastos

3. ACCIONES DE TURNO (si es tu turno):
   - Botón: "CAMBIAR con [nombre jugador derecha]"
   - Botón alternativo (último jugador): "CAMBIAR con MAZO"
   - Botón: "QUEDARSE" (no cambiar)
   
4. INDICADOR DE ACCIÓN:
   - Animación cuando alguien cambia cartas
   - Toast/snackbar: "[Jugador] ha cambiado con [otro]"
   - Animación de cartas intercambiándose

5. MAZO CENTRAL:
   - Pila de cartas en centro (decorativo)
   - Se ilumina si el último jugador puede robar

ESTADO DE RONDA:
- Al final: todas las cartas se voltean con animación
- Perdedor resaltado en rojo
- Mostrar "¡Se queda sin vida!" si aplica

ESTILO:
- Fondo: tapete de juego (verde oscuro o #1A1A2E)
- Cartas: diseño realista con sombras
- Animaciones: flip de cartas, intercambio fluido
- Indicador de turno: glow amarillo (#FFD93D)
- Feedback sonoro/háptico en acciones

DIMENSIONES: 360x800dp
```

---

## 7️⃣ Vista: El As - Resultado de Ronda (ElAsRoundResultView)

### Prompt para Stitch:

```
Diseña la pantalla de resultado tras una ronda de "El As".

CONTENIDO:

1. TÍTULO:
   - "Resultado de la Ronda [X]"

2. GRID DE RESULTADOS:
   - Mostrar todos los jugadores en grid vertical
   - Cada jugador:
     · Avatar
     · Nombre
     · Carta que tenía (revelada)
     · Vidas restantes
   - Perdedor destacado:
     · Fondo rojo (#FF4757)
     · Texto: "PIERDE 1 VIDA"
     · Animación de corazón rompiéndose

3. SI HAY ELIMINADOS:
   - Sección especial:
     · "[Jugador] ha sido ELIMINADO"
     · Avatar en gris/desaturado
     · Texto: "0 vidas restantes"

4. ESTADÍSTICAS:
   - Jugadores restantes: X/Y
   - Ronda actual

FOOTER:
- Si quedan 2+ jugadores:
  · Botón primario: "SIGUIENTE RONDA"
- Si hay ganador (1 jugador):
  · Texto grande: "¡[Nombre] GANA!"
  · Animación de victoria
  · Botón: "JUGAR DE NUEVO"
  · Botón secundario: "VOLVER AL HUB"

ESTILO:
- Fondo: gradiente oscuro
- Cards por jugador con glassmorphism
- Perdedor: efecto shake + color rojo
- Ganador final: confetti + colores dorados
- Animaciones de entrada escalonadas

DIMENSIONES: 360x800dp
```

---

## 8️⃣ Vista: Ajustes (SettingsView)

### Prompt para Stitch:

```
Diseña una pantalla de ajustes/configuración simple para la app de juegos.

HEADER:
- Botón atrás (flecha) esquina superior izquierda
- Título: "Ajustes"

CONTENIDO - LISTA DE OPCIONES:

1. SECCIÓN "Audio":
   - Switch: "Sonido" (ON/OFF)
   - Switch: "Vibración" (ON/OFF)
   - Slider: "Volumen" (0-100)

2. SECCIÓN "Perfil":
   - Input de texto: "Tu nombre"
   - Texto pequeño: "Se mostrará a otros jugadores en LAN"

3. SECCIÓN "Red":
   - Toggle: "Permitir descubrimiento LAN" (ON/OFF)
   - Texto info: Estado de WiFi (Conectado/Desconectado)

4. SECCIÓN "Acerca de":
   - Texto: "Versión 1.0.0"
   - Botón texto: "Ver código fuente"
   - Botón texto: "Licencias"

5. SECCIÓN "Datos":
   - Botón destructivo: "Borrar estadísticas"
   - Texto pequeño: "Esto no se puede deshacer"

ESTILO:
- Fondo: #1A1A2E
- Lista de opciones en cards separadas
- Switches estilo Material Design
- Sliders con thumb destacado
- Inputs con underline
- Texto destructivo en rojo
- Separadores sutiles entre secciones

DIMENSIONES: 360x800dp
```

---

## 9️⃣ Vista: Descubrimiento de Salas LAN (RoomDiscoveryView)

### Prompt para Stitch:

```
Diseña una pantalla para descubrir y unirse a salas LAN.

HEADER:
- Botón atrás (flecha) esquina superior izquierda
- Título: "Unirse a Sala"
- Icono de refresh (recargar) esquina superior derecha

MÉTODOS DE UNIÓN:

1. UNIRSE CON CÓDIGO:
   - Card destacada en la parte superior
   - Input de código: 6 dígitos grandes
   - Botón: "UNIRSE"
   - Placeholder: "ABCD12"

2. SALAS DISPONIBLES:
   - Subtítulo: "Salas en tu red WiFi"
   - Lista vertical scrollable de salas encontradas
   - Cada sala:
     · Nombre de la sala
     · Juego (icono + nombre)
     · Jugadores: "3/8"
     · Host: nombre del creador
     · Indicador de señal (WiFi strength)
     · Botón: "UNIRSE"
   
   - Si no hay salas:
     · Ilustración vacía (WiFi + signo de interrogación)
     · Texto: "No se encontraron salas"
     · Texto pequeño: "Asegúrate de estar en la misma red WiFi"

3. INDICADOR DE BÚSQUEDA:
   - Spinner animado mientras busca
   - Texto: "Buscando salas..."

ESTADO SIN WIFI:
- Alert destacada en amarillo
- Icono de WiFi desconectado
- Texto: "No hay conexión WiFi"
- Subtexto: "Conéctate a una red para jugar en LAN"

ESTILO:
- Fondo: gradiente #1A1A2E
- Input de código: grande, centrado, monospace
- Cards de salas con glassmorphism
- Indicador de jugadores: progress bar
- Animación de refresh al recargar
- Empty state con ilustración friendly

DIMENSIONES: 360x800dp
```

---

## 🎨 Recursos Necesarios

Para implementar estas vistas, necesitarás:

### Iconos
- `ic_settings` - Engranaje
- `ic_arrow_back` - Flecha atrás
- `ic_brain` - Cerebro (The Mind)
- `ic_cards` - Cartas (El As)
- `ic_phone` - Teléfono (modo local)
- `ic_wifi` - WiFi/red (modo LAN)
- `ic_heart` - Corazón (vidas)
- `ic_menu` - Tres puntos
- `ic_refresh` - Recargar
- `ic_check` - Check/confirmación
- `ic_error` - Error/advertencia
- `ic_crown` - Corona (host)

### Ilustraciones
- Empty state (sin salas)
- Confetti/celebración
- WiFi desconectado

### Animaciones
- Ripple effects en botones
- Transitions entre pantallas
- Card flip para cartas
- Number reveal animations
- Loading spinners
- Confetti particles

---

**Notas Importantes:**
1. Todas las vistas deben seguir la paleta de colores definida
2. Mantener consistencia en corner radius (16-24dp)
3. Espaciado estándar: 8dp, 16dp, 24dp
4. Tamaño mínimo de botones: 48x48dp
5. Tipografía legible: mínimo 14sp
6. Soporte para modo oscuro nativo
7. Todos los textos en español

---

**Última actualización**: 6 de Febrero de 2026
