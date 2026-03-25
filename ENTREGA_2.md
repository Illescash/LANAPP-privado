# Entrega 2 - PartyHub 🎮

**Asignatura:** Desarrollo de Aplicaciones para Dispositivos Móviles (DADM)
**Curso:** 2025-2026 · **Docente:** Alicia Garrido Peña
**Equipo:** Rafael Romero Monzón · Diego Illescas Lasa
**Fecha entrega:** 8 de abril de 2026 · **Peso:** 30% de la nota final

> 📋 Ver rúbrica detallada y contenido del curso en [`ENTREGA2/NORMAS_ENTREGA2.md`](./ENTREGA2/NORMAS_ENTREGA2.md)

---

## Alcance de la Entrega

Hub funcional + minijuegos en **modo LOCAL** (un solo dispositivo). Se evalúa la capacidad de aplicar los conceptos del curso (Módulos 4-7) al proyecto real.

---

## Criterios de Evaluación (Resumen)

| Criterio | Peso | Cómo lo cubrimos en PartyHub |
|----------|------|------------------------------|
| Entorno de desarrollo | 0,70 | Proyecto Android Studio con Gradle configurado, versionado correcto |
| Recursos y diseño | 1,50 | Layouts XML (ConstraintLayout), strings.xml, colors.xml, icono y nombre personalizados |
| Escuchadores | 0,80 | Botones del Hub, acciones de juego (jugar carta, seleccionar número) |
| DataBinding | 1,25 | ViewBinding en Activities/Fragments, DataBinding con `@{}` en vistas de juego |
| Intenciones y Navegación | 1,50 | Intent Hub→Juego, NavGraph entre Fragments de configuración/juego/resultados |
| Ciclo de vida | 1,00 | Guardar estado de partida en onSaveInstanceState, restaurar en onCreate |
| ViewModel | 1,25 | GameViewModel con StateFlow, MindGameState/AsGameState como LiveData |
| Control de versiones | 1,00 | Plan de commits estructurado (ver sección abajo) |
| Aplicación general | 1,00 | Coherencia y calidad del producto final |

---

## Arquitectura de la Entrega 2

```
HubActivity
├── HubFragment (selección de juego)
│
├── [Intent explícito] → TheMindActivity
│   ├── MindConfigFragment (num jugadores, dificultad)
│   ├── MindGameFragment (pantalla de juego)
│   └── MindResultFragment (resultados)
│   └── MindViewModel + MindGameEngine
│
└── [Intent explícito] → ElAsActivity
    ├── AsConfigFragment (num jugadores)
    ├── AsGameFragment (pantalla de juego)
    └── AsResultFragment (resultados)
    └── AsViewModel + AsGameEngine
```

---

## Plan de Commits (25 marzo → 8 abril)

**Convención de mensajes:**
```
tipo(ámbito): descripción breve

feat(hub): añadir pantalla principal con selección de juegos
fix(mind): corregir lógica de validación de nivel
docs(readme): actualizar documentación de la entrega 2
refactor(as): extraer lógica de barajeo a función separada
style(hub): ajustar colores y tipografía del menú principal
chore(gradle): configurar versionado y dependencias
```

### Semana 1 (25-31 marzo) — Fundamentos

| Fecha | Responsable | Commits planificados |
|-------|-------------|---------------------|
| 25 mar | Diego | `chore(project): inicializar proyecto Android Studio` · `chore(gradle): configurar dependencias (ViewBinding, Navigation, ViewModel)` |
| 26 mar | Rafael | `feat(hub): crear HubActivity con layout base` · `style(resources): definir strings.xml, colors.xml, dimens.xml` |
| 27 mar | Diego | `feat(hub): implementar lista de juegos con RecyclerView` · `feat(hub): añadir navegación a juegos con Intent` |
| 28 mar | Rafael | `feat(mind): crear MindActivity y NavGraph` · `feat(mind): implementar MindConfigFragment` |
| 29 mar | Diego | `feat(mind): implementar MindGameFragment con UI` · `feat(mind): añadir DataBinding a vista de juego` |
| 30 mar | Rafael | `feat(mind): crear MindViewModel con StateFlow` · `feat(mind): implementar MindGameEngine (lógica pura)` |
| 31 mar | Ambos | `feat(mind): conectar ViewModel ↔ Engine ↔ UI` · `fix(mind): manejar ciclo de vida (onSaveInstanceState)` |

### Semana 2 (1-8 abril) — El As + Pulido

| Fecha | Responsable | Commits planificados |
|-------|-------------|---------------------|
| 1 abr | Diego | `feat(as): crear AsActivity y NavGraph` · `feat(as): implementar AsConfigFragment` |
| 2 abr | Rafael | `feat(as): implementar AsGameFragment con UI` · `feat(as): añadir DataBinding a cartas` |
| 3 abr | Diego | `feat(as): crear AsViewModel con StateFlow` · `feat(as): implementar AsGameEngine` |
| 4 abr | Rafael | `feat(as): conectar ViewModel ↔ Engine ↔ UI` · `fix(as): manejar ciclo de vida` |
| 5 abr | Diego | `feat(results): crear pantallas de resultados (Mind + As)` · `style(app): personalizar icono y nombre` |
| 6 abr | Rafael | `refactor(binding): justificar uso de DataBinding vs ViewBinding` · `docs(entrega): documentar decisiones de diseño` |
| 7 abr | Ambos | `style(ui): pulir diseño Material Design` · `fix(lifecycle): testing de rotación/background` |
| 8 abr | Ambos | `docs(readme): actualizar documentación final` · `chore(release): preparar versión de entrega` |

### Ramas sugeridas
```
main
├── develop
│   ├── feature/hub
│   ├── feature/the-mind
│   ├── feature/el-as
│   └── feature/ui-polish
```

---

## Estructura del Proyecto

```
LANAPP-privado/
├── 00-idea-y-concepto/          # Concepto y visión
├── 01-requisitos/               # RF, RNF, Casos de uso
├── 02-diseño/                   # Arquitectura, modelo de datos
├── 03-diseño-ui/                # Mockups y prompts
├── ENTREGA2/
│   └── NORMAS_ENTREGA2.md       # Rúbrica + contenido curso (referencia densa)
├── app/                         # ← Proyecto Android Studio (por crear)
│   └── src/main/
│       ├── java/com/partyhub/
│       │   ├── core/            # Base classes, DI
│       │   ├── feature/hub/     # Hub principal
│       │   ├── feature/themind/ # The Mind (Fragment, VM, Engine)
│       │   └── feature/elas/   # El As (Fragment, VM, Engine)
│       └── res/                 # Layouts, strings, colors, drawables
├── CLAUDE.md                    # Contexto técnico para agentes
├── ENTREGA_1.md                 # Documento Entrega 1
├── ENTREGA_2.md                 # Este archivo
└── README.md                    # Resumen del proyecto
```
