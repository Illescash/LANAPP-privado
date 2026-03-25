# Normas y Contenido - Entrega 2 (DADM 2025-2026)

> **Fuente**: Curso UAMx + Rúbrica oficial de evaluación
> **Entrega**: 8 de abril de 2026 | **Peso**: 30% de la nota final
> **Equipo**: Rafael Romero Monzón · Diego Illescas Lasa

---

## Rúbrica de Evaluación (10 puntos)

| # | Criterio | Peso | Desglose |
|---|----------|------|----------|
| 1 | **Entorno de desarrollo** | 0,70 | Cambiar versiones fácilmente (0,20) · Conocer significado/utilidad de cada elemento del proyecto Android (0,50) |
| 2 | **Recursos y fichero de diseño** | 1,50 | Justificar tipo de vista y posicionamiento (0,75) · Nombre e icono personalizados (0,25) · Cambio ágil de strings/recursos (0,50) · Calidad de vistas (0,25) |
| 3 | **Escuchadores** | 0,80 | Funcionamiento correcto de botones (0,50) · Escalabilidad ante cambios (0,30) |
| 4 | **DataBinding** | 1,25 | Implementación correcta (0,25) · Justificar cuándo usarlo y cuándo no (0,50) · Describir ventajas del cambio (0,50) |
| 5 | **Intenciones y Navegación** | 1,50 | Implementación correcta (0,60) · Distintos tipos de intenciones (0,30) · Justificar posición/motivo (0,60) |
| 6 | **Ciclo de vida** | 1,00 | Describir ciclo en cada estado (0,60) · Implementación correcta (0,40) |
| 7 | **ViewModel** | 1,25 | Modelos + relación con ViewModel (0,75) · Aplicar modificaciones similares con comprensión del paradigma (0,50) |
| 8 | **Control de versiones** | 1,00 | Commits frecuentes/pequeños (0,40) · Mensajes descriptivos/estructurados (0,40) · Uso de ramas (0,20) |
| — | **Aplicación de conocimientos** | 1,00 | Aplicación general de lo aprendido al desarrollo |
| | **TOTAL** | **10,00** | |

---

## Contenido del Curso UAMx (Mapa de Conocimientos)

### Módulo 1 — Información General
- Primera presentación (días 20 y 25)
- Android Awards DADM 2026
- Planificación del curso

### Módulo 2 — Introducción a Kotlin
| Tema | Contenido clave |
|------|----------------|
| Instalando el entorno | JDK, Android Studio, Kotlin playground |
| Variables anulables y no anulables | `var`/`val`, `String?` vs `String`, operador `?.`, `!!`, `?:` (Elvis) |
| Colecciones y condicionales | `List`, `MutableList`, `Map`, `Set` · `when`, `if` como expresión |
| Funciones y excepciones | `fun`, parámetros por defecto/nombrados, `try/catch`, funciones de extensión |
| Clases | `data class`, `sealed class`, `enum class`, herencia, `object`, companion object |
| Funciones de contexto | `let`, `apply`, `run`, `with`, `also` — scoping functions |

### Módulo 3 — Patrón MVVM
| Tema | Contenido clave |
|------|----------------|
| Diseñando una App en Android | Separación de responsabilidades: Model ↔ View ↔ ViewModel |
| Primera entrega: Diseño de la app y requisitos | Definición de requisitos funcionales, casos de uso, modelo de datos |

### Módulo 4 — Introducción a Android Studio
| Tema | Contenido clave |
|------|----------------|
| Documentación y bibliografía | developer.android.com, Kotlin docs |
| Instalación de Android Studio y dispositivos | SDK Manager, AVD, dispositivos físicos (USB debugging) |
| El entorno de desarrollo | Project structure, Gradle, módulos, `build.gradle.kts` |
| La app "Playground" | Proyecto de pruebas para experimentar con APIs |
| Creando app propia | Crear proyecto desde template, configurar `minSdk`, `targetSdk` |

### Módulo 5 — Elementos de una Actividad ⭐ (Core de la Entrega 2)
| Tema | Contenido clave | Rúbrica |
|------|----------------|---------|
| Actividades en Android | `Activity`, `AppCompatActivity`, `onCreate()`, `setContentView()` | → Criterio 1 |
| Recursos | `res/layout`, `res/values` (strings.xml, colors.xml, dimens.xml), `res/drawable` | → Criterio 2 |
| Elementos de diseño | XML layouts: `ConstraintLayout`, `LinearLayout`, `FrameLayout`, vistas (`TextView`, `ImageView`, `Button`) | → Criterio 2 |
| Elementos de actividad | `AndroidManifest.xml`, permisos, intent-filters, configuración de Activity | → Criterio 1 |
| Elementos Interactivos | `OnClickListener`, `RecyclerView`, `Adapter`, `ViewHolder`, `FloatingActionButton` | → Criterio 3 |
| DataBinding | `viewBinding` vs `dataBinding`, `binding.variable`, `@{}` en XML, `LiveData` con binding | → Criterio 4 |
| Desarrollando el proyecto | Integración de todos los conceptos anteriores en el proyecto real | → General |

### Módulo 6 — Actividades vs Fragmentos ⭐ (Core de la Entrega 2)
| Tema | Contenido clave | Rúbrica |
|------|----------------|---------|
| Logging | `Log.d()`, `Log.e()`, `Log.i()`, Logcat, filtros | → Debugging |
| Intenciones: Navegando entre actividades | `Intent` explícito/implícito, `putExtra()`, `startActivity()`, `ActivityResultLauncher` | → Criterio 5 |
| Ciclo de Vida de una Aplicación | `onCreate` → `onStart` → `onResume` → `onPause` → `onStop` → `onDestroy`, `onSaveInstanceState` | → Criterio 6 |
| Persistencia | `SharedPreferences`, `Room` (SQLite wrapper), `DataStore` | → Criterio 6 |
| Fragmentos | `Fragment`, `FragmentManager`, `FragmentTransaction`, `viewLifecycleOwner` | → Criterio 5, 7 |
| Navegación entre fragmentos | Jetpack Navigation: `NavGraph`, `NavController`, `NavHostFragment`, `SafeArgs` | → Criterio 5 |

### Módulo 7 — Desarrollo del Proyecto
- ViewModel: `ViewModel`, `ViewModelProvider`, `LiveData`, `MutableLiveData`, `StateFlow` → **Criterio 7**
- Integración final de todos los conceptos

---

## Qué Debe Tener la Entrega (Checklist Derivado)

### Obligatorio para aprobar
- [ ] Proyecto Android Studio compilable y ejecutable
- [ ] Nombre e icono personalizados de la app
- [ ] Al menos 2 Activities o 1 Activity + Fragments con navegación
- [ ] Uso de recursos externalizados (strings.xml, colors.xml)
- [ ] DataBinding o ViewBinding implementado
- [ ] Al menos 1 Intent (explícito o implícito)
- [ ] Manejo del ciclo de vida (guardar/restaurar estado)
- [ ] ViewModel con LiveData o StateFlow
- [ ] Commits regulares en Git con mensajes descriptivos

### Para nota alta
- [ ] Justificación documentada de decisiones de diseño (layouts, binding, etc.)
- [ ] Múltiples tipos de intenciones (explícitas + implícitas)
- [ ] Persistencia con SharedPreferences o Room
- [ ] Uso de ramas en Git (feature branches)
- [ ] Logging con Logcat para debugging
- [ ] UI pulida con Material Design Components
