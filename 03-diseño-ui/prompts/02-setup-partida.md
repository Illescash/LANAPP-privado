

## Tarea
Diseña una pantalla de configuración de partida para una app de juegos móvil.

## Header
- Botón atrás (flecha izquierda) en esquina superior izquierda.
- Título del juego seleccionado centrado.
- Altura: ~56dp.

## Contenido Principal

1. **SECCIÓN "Modo de Juego"**:
   - Dos botones grandes tipo toggle:
     - "LOCAL" (Mismo dispositivo, icono de teléfono único).
     - "LAN" (Red WiFi, icono de varios dispositivos conectados).
   - Botón seleccionado con color primario (#6C63FF).
   - Botón no seleccionado con borde outline.

2. **SECCIÓN "Jugadores"**:
   - **Si modo LOCAL**: Selector numérico (spinner/stepper) de 2 a 8 jugadores con botones +/- grandes.
   - **Si modo LAN**: Mostrar "Esperando jugadores..." con spinner y lista de jugadores conectados.

3. **SECCIÓN "Opciones"**:
   - Input de texto: "Tu nombre".
   - Switch: "Sonido" (ON/OFF).
   - Switch: "Vibración" (ON/OFF).

## Footer
- Botón CTA grande: "COMENZAR PARTIDA" (modo local) o "CREAR SALA" (modo LAN).
- Botón secundario outline: "UNIRSE A SALA" (solo en modo LAN).

## Estilo Específico
- Fondo: #1A1A2E.
- Secciones separadas con cards (#16213E).
- Botón principal: degradado de #6C63FF a #5548E8.
- Espaciado vertical: 16dp entre secciones.
- Padding lateral: 20dp.
