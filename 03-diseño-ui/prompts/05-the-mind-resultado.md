# Prompt: The Mind - Resultado (MindResultView)

Utiliza la configuración de estilo de `00-configuracion-general.md`.

## Tarea
Diseña la pantalla de transición tras completar o fallar un nivel en "The Mind".

## Diseño del Resultado

1. **MENSAJE CENTRAL**:
   - **ÉXITO**: Icono check animado, texto "¡Nivel Completado!", color verde (#00D9A3) con efecto glow.
   - **FALLO**: Icono corazón roto, texto "Nivel Fallido", color rojo (#FF4757).

2. **RESUMEN DE RONDA**:
   - Timeline con todos los números jugados en esta ronda.
   - Resalte visual del punto de error si lo hubo.

3. **ESTADO GLOBAL**:
   - Vidas restantes (corazones).
   - Progreso de niveles.

## Footer
- **Éxito (y hay vidas)**: Botón "SIGUIENTE NIVEL".
- **Sin vidas**: Texto "Fin del Juego", botón "JUGAR DE NUEVO" y botón "VOLVER AL HUB".

## Estilo
- Animaciones de entrada tipo "scale in".
- Card central con glassmorphism.
- Fondo con gradiente dinámico según el resultado (más verdoso o más rojizo/oscuro).
