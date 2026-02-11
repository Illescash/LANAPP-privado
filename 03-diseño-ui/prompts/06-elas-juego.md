# Prompt: El As - Pantalla de Juego (ElAsGameView)

Utiliza la configuración de estilo de `00-configuracion-general.md`.

## Tarea
Diseña la pantalla de juego para "El As", basada en una baraja española.

## Header
- Info de "Ronda [X]" y vidas de todos los jugadores.

## Tablero Principal
- Distribución circular de los avatares de los jugadores.
- Glow amarillo en el borde del avatar del jugador que tiene el turno.

## Tu Carta (Zona Inferior)
- **Modo LOCAL**: Carta boca abajo con botón "VER MI CARTA".
- **Modo LAN**: Carta boca arriba (visibilidad total para el dueño).
- Diseño de carta realista (baraja española, valores 1-12).

## Acciones de Turno
- Botón "CAMBIAR con [Jugador Derecha]".
- Botón "CAMBIAR con MAZO" (solo último jugador).
- Botón "QUEDARSE".

## Estilo Específico
- Fondo tipo tapete de juego (verde oscuro premium o azul muy oscuro).
- Animaciones fluidas de intercambio de cartas.
- Mazo central decorativo que reacciona cuando es opción de robo.
- Toasts informativos: "[Nombre] ha cambiado su carta".
