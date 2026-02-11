# Prompt: Sala LAN (LANLobbyView)

Utiliza la configuración de estilo de `00-configuracion-general.md`.

## Tarea
Diseña una pantalla de sala de espera multijugador LAN.

## Header
- Botón "Salir" (X) en esquina superior izquierda.
- Título: "Sala de [Nombre del juego]".
- Código de sala (6 dígitos) debajo del título.

## Contenido Principal

1. **INFORMACIÓN DE LA SALA**:
   - Nombre de la sala y juego seleccionado.
   - Host identificado con una corona o badge.

2. **LISTA DE JUGADORES**:
   - Grid vertical de jugadores conectados (máx 8).
   - Cada jugador: Avatar circular (color único), nombre, badge "HOST" si aplica, punto verde de conexión.
   - Espacios vacíos: Placeholder gris con icono de usuario y texto "Esperando...".

3. **INDICADOR DE ESTADO**:
   - Si eres host: "Esperando jugadores..."
   - Si eres cliente: "Esperando que el host inicie..."
   - Animación de puntos suspensivos.

## Footer
- **Si eres HOST**: Botón primario "INICIAR PARTIDA" (habilitado con 2+ jugadores).
- **Si eres CLIENTE**: Texto descriptivo informando que el host iniciará.

## Detalles Extras
- Botón "Copiar código" junto al código de sala.
- Estilo con glassmorphism en la card central.
- Código de sala grande y en fuente monoespaciada.
