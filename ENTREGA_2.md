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
| Recursos y diseño | 1,50 | Layouts XML (RelativeLayout/LinearLayout), strings.xml, colors.xml, icono y nombre personalizados |
| Escuchadores | 0,80 | Botones del Hub, acciones de juego (jugar carta, seleccionar número) |
| DataBinding | 1,25 | ViewBinding en Activities, DataBinding con `@{}` en Fragments de juego |
| Intenciones y Navegación | 1,50 | Intent explícito Hub→Juego, Intent implícito (compartir resultado), NavGraph + Safe Args entre Fragments |
| Ciclo de vida | 1,00 | Guardar estado de partida en onSaveInstanceState, restaurar en onCreate |
| ViewModel | 1,25 | GameViewModel con MutableLiveData/LiveData (backing property pattern) |
| Control de versiones | 1,00 | Plan de commits estructurado (ver sección abajo) |
| Aplicación general | 1,00 | Coherencia y calidad del producto final |

---

## Arquitectura de la Entrega 2

```
HubActivity
├── HubFragment (selección de juego con LinearLayout)
│
├── [Intent explícito] → TheMindActivity
│   ├── NavGraph + Safe Args
│   ├── MindConfigFragment (num jugadores, dificultad)
│   ├── MindGameFragment (pantalla de juego con DataBinding @{})
│   └── MindResultFragment (resultados + Intent implícito compartir)
│   └── MindViewModel (LiveData) + MindGameEngine (lógica pura)
│
└── [Intent explícito] → ElAsActivity
    ├── NavGraph + Safe Args
    ├── AsConfigFragment (num jugadores)
    ├── AsGameFragment (pantalla de juego con DataBinding @{})
    └── AsResultFragment (resultados + Intent implícito compartir)
    └── AsViewModel (LiveData) + AsGameEngine (lógica pura)
```

### Patrones técnicos alineados con UAMx

- **ViewModel**: `ViewModelProvider(this).get(...)` con `by lazy` (sin Hilt)
- **LiveData**: `MutableLiveData` privado + `LiveData` público (backing property)
- **Fragments**: `viewLifecycleOwner` para observar, patrón `_binding`/`binding` con limpieza en `onDestroyView()`
- **DataBinding**: `@{}` en XML para vincular datos del ViewModel, `binding.lifecycleOwner = viewLifecycleOwner`
- **Selección de juegos**: LinearLayout con botones (sin RecyclerView, no visto en clase)
- **Intenciones**: explícitas (Hub→Juego) + implícitas (`ACTION_SEND` para compartir resultados)
- **Safe Args**: para pasar configuración entre fragments (num jugadores, dificultad)

---

## Plan de Commits (25 marzo → 8 abril)

**Convención de mensajes** (Conventional Commits, como indica la profe en UAMx):
```
tipo(ámbito): descripción breve

feat(hub): añadir pantalla principal con selección de juegos
fix(mind): corregir lógica de validación de nivel
docs(readme): actualizar documentación de la entrega 2
refactor(as): extraer lógica de barajeo a función separada
style(hub): ajustar colores y tipografía del menú principal
chore(gradle): configurar versionado y dependencias
```

### Commit inicial

| Fecha | Responsable | Commit | Estado |
|-------|-------------|--------|--------|
| 28 mar | Diego | `chore(project): setup inicial del proyecto Android con Hilt y HubActivity` | ✅ d07eb9c |

### Semana 1 (28-31 marzo) — Hub + The Mind

| Fecha | Responsable | Commits planificados | Estado |
|-------|-------------|---------------------|--------|
| 28 mar | Diego | `refactor(project): reemplazar Hilt por ViewModelProvider` · `chore(gradle): configurar DataBinding, Navigation y Safe Args` | ✅ 4539e6b (Rafael) |
| 29 mar | Rafael | `feat(hub): crear HubFragment con selección de juegos` · `style(resources): definir strings.xml, colors.xml, dimens.xml` | ✅ 7e31a29 |
| 29 mar | Diego | `feat(hub): añadir navegación Hub→Juegos con Intent explícito` | ✅ a2de385 |
| 30 mar | Rafael | `feat(mind): crear MindActivity y NavGraph con Safe Args` · `feat(mind): implementar MindConfigFragment con DataBinding` | ⬜ |
| 30 mar | Diego | `feat(mind): implementar MindGameEngine (lógica pura Kotlin)` · `feat(mind): crear MindViewModel con LiveData` | ✅ 098e4c4 |
| 31 mar | Ambos | `feat(mind): implementar MindGameFragment con DataBinding` · `feat(mind): conectar ViewModel ↔ Engine ↔ UI` | ⬜ |

### Semana 2 (1-8 abril) — El As + Pulido + Entrega

| Fecha | Responsable | Commits planificados | Estado |
|-------|-------------|---------------------|--------|
| 1 abr | Diego | `feat(as): crear AsActivity y NavGraph con Safe Args` · `feat(as): implementar AsConfigFragment con DataBinding` | ⬜ |
| 2 abr | Rafael | `feat(as): implementar AsGameEngine (lógica pura Kotlin)` · `feat(as): crear AsViewModel con LiveData` | ⬜ |
| 3 abr | Ambos | `feat(as): implementar AsGameFragment con DataBinding` · `feat(as): conectar ViewModel ↔ Engine ↔ UI` | ⬜ |
| 4 abr | Diego | `feat(results): crear pantallas de resultados (Mind + As)` · `feat(results): añadir intent implícito para compartir resultado` | ⬜ |
| 5 abr | Rafael | `fix(lifecycle): guardar/restaurar estado con onSaveInstanceState` · `style(app): personalizar icono y nombre de la app` | ⬜ |
| 6 abr | Ambos | `style(ui): pulir diseño y recursos (colors, strings, dimens)` · `feat(hub): añadir logging con Timber` | ⬜ |
| 7 abr | Ambos | `fix(lifecycle): testing de rotación y segundo plano` · `refactor(binding): justificar DataBinding vs ViewBinding en código` | ⬜ |
| 8 abr | Ambos | `docs(readme): documentación final` · `chore(release): preparar versión de entrega` | ⬜ |

### Ramas sugeridas
```
main
├── develop
│   ├── feature/hub
│   ├── feature/the-mind
│   ├── feature/el-as
│   └── feature/ui-polish
```

### Diferido a Entrega 3

Lo siguiente **no se incluye** en E2 por no haberse visto en clase o no ser necesario para modo LOCAL:
- Hilt / DI (se usa ViewModelProvider directamente)
- Coroutines + Flow + StateFlow (se usa LiveData)
- RecyclerView (se usa LinearLayout; RecyclerView no visto en UAMx)
- Jetpack Compose (E3)
- kotlinx.serialization (E3, para protocolo LAN)
- Red LAN TCP/UDP (E3)
- Clean Architecture con Repository (E3)

---

## Estructura del Proyecto

```
LANAPP-privado/ (repo documentación)
├── 00-idea-y-concepto/          # Concepto y visión
├── 01-requisitos/               # RF, RNF, Casos de uso
├── 02-diseño/                   # Arquitectura, modelo de datos
├── 03-diseño-ui/                # Mockups y prompts
├── ENTREGA2/
│   ├── NORMAS_ENTREGA2.md       # Rúbrica + contenido curso
│   └── UAMx.txt                 # Contenido completo del curso UAMx
├── CLAUDE.md                    # Contexto técnico para agentes
├── ENTREGA_1.md                 # Documento Entrega 1
├── ENTREGA_2.md                 # Este archivo (plan de commits)
└── README.md                    # Resumen del proyecto

app/ (repo Android Studio — separado, en .gitignore del padre)
└── src/main/
    ├── java/com/partyhub/
    │   ├── core/model/          # Data classes (Player, Card)
    │   ├── feature/hub/         # HubActivity, HubFragment
    │   ├── feature/themind/     # MindActivity, Fragments, VM, Engine
    │   └── feature/elas/        # AsActivity, Fragments, VM, Engine
    └── res/
        ├── layout/              # activity_*.xml, fragment_*.xml
        ├── navigation/          # nav_mind.xml, nav_as.xml
        ├── values/              # strings.xml, colors.xml, dimens.xml, themes.xml
        ├── drawable/            # Iconos y gráficos
        └── mipmap/              # Launcher icons
```
