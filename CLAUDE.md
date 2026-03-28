# PartyHub - Contexto del Proyecto

## Qué es

PartyHub es una app Android (Kotlin) que funciona como hub de minijuegos para jugar con amigos de forma presencial. Soporta dos modos: **Local** (un solo dispositivo) y **LAN** (cada jugador en su dispositivo, sin internet).

## Equipo y Entregas

| | |
|-|-|
| **Equipo** | Rafael Romero Monzón · Diego Illescas Lasa |
| **Asignatura** | DADM – UAM EPS 2025-2026 · Alicia Garrido Peña |

| Entrega | Peso | Contenido | Fecha | Estado |
|---------|------|-----------|-------|--------|
| E1 | — | Diseño + requisitos (presentación) | Completada | ✅ |
| E2 | 30% | Hub + juegos en modo LOCAL | 8 abril 2026 | 🔨 En curso |
| E3 | 70% | Añadir modo LAN + presentación | TBD | ⏳ |

### Entrega 2 — Referencia rápida
- **Rúbrica y normas detalladas**: [`ENTREGA2/NORMAS_ENTREGA2.md`](./ENTREGA2/NORMAS_ENTREGA2.md)
- **Plan de entrega y commits**: [`ENTREGA_2.md`](./ENTREGA_2.md)
- Criterios clave: Entorno (0,70) · Recursos/diseño (1,50) · Escuchadores (0,80) · DataBinding (1,25) · Intenciones/Nav (1,50) · Ciclo vida (1,00) · ViewModel (1,25) · Git (1,00) · General (1,00)

## Juegos incluidos

| Juego | Mecánica | Jugadores |
|-------|----------|-----------|
| **The Mind** | Cooperativo: ordenar números del 1-100 sin comunicarse | 2-8 |
| **El As** | Baraja española: intercambiar cartas a ciegas, sobrevivir con 3 vidas | 3-8 |

## Stack técnico

### Entrega 2 (actual) — Solo lo visto en UAMx
- **Lenguaje**: Kotlin
- **UI**: XML layouts + ViewBinding (Activities) + DataBinding con `@{}` (Fragments de juego)
- **Arquitectura**: MVVM (ViewModel + LiveData + GameEngine)
- **ViewModel**: `ViewModelProvider(this).get(...)` con `by lazy` — **sin Hilt**
- **Datos reactivos**: `MutableLiveData` (privado) + `LiveData` (público) — backing property pattern
- **Navegación**: Jetpack Navigation Component (NavGraph + Safe Args)
- **Intenciones**: explícitas (Hub→Juego) + implícitas (`ACTION_SEND` compartir resultado)
- **Selección de juegos**: LinearLayout con botones — **sin RecyclerView** (no visto en clase)
- **Logging**: Timber
- **Min SDK**: Android 7.0 (API 24)

### Entrega 3 (futuro) — Se añadirá
- Jetpack Compose (sustituir XML layouts)
- Hilt / DI
- Coroutines + Flow + StateFlow
- RecyclerView
- kotlinx.serialization (JSON)
- Red LAN: TCP (juego) + UDP broadcast (descubrimiento)
- Clean Architecture con Repository

## Arquitectura y flujo de datos (E2)

```
View (Fragment con DataBinding) → ViewModel (LiveData) → GameEngine (lógica pura Kotlin)
```

En modo LAN (E3), el ViewModel despachará acciones al `NetworkManager`. El Host tendrá la GameEngine "verdadera" (fuente de verdad).

### Componentes clave (E2)

- **`GameEngine`** (`feature/[game]/engine/`): Lógica pura del juego. 100% Kotlin, 0% Android. Recibe acciones, devuelve estado.
- **`ViewModel`** (`feature/[game]/`): Coordina UI con GameEngine. Usa `MutableLiveData`/`LiveData`. Instanciado con `ViewModelProvider` + `by lazy`.
- **`Fragments`**: Usan `viewLifecycleOwner`, patrón `_binding`/`binding`, limpieza en `onDestroyView()`.
- **`NetworkManager`** (`network/`, solo E3): Abstrae TCP/UDP. Roles: Host vs Client.

### Estructura de paquetes (`com.partyhub`)

```
core/model/       → Data classes inmutables (Player, Card). Sin lógica.
feature/hub/      → Pantalla principal (HubActivity, HubFragment)
feature/themind/  → Juego The Mind (Activity, Fragments, VM, Engine)
feature/elas/     → Juego El As (Activity, Fragments, VM, Engine)
```

## Dos repositorios Git

Este proyecto usa **dos repos independientes**:

| Repo | Ruta | Contenido |
|------|------|-----------|
| **Documentación** | raíz (`Apps móviles/`) | Docs, diseño, requisitos, prototipos, plan de entrega |
| **App Android** | `app/` | Proyecto Android Studio completo (código fuente evaluable) |

`app/` está en el `.gitignore` del repo padre para evitar conflictos. Son repos con historiales, ramas y remotos separados. Los commits del plan de entrega (`ENTREGA_2.md`) van al repo de `app/`.

## Reglas para agentes

### Código
- **GameEngine NUNCA debe importar `android.*`** — es lógica pura Kotlin.
- **ViewBinding** en Activities. **DataBinding** con `@{}` en Fragments que observan datos del ViewModel.
- Los Fragments deben usar `viewLifecycleOwner` para observar LiveData (NO `this`).
- Fragments: usar patrón `_binding` (nullable) / `binding` (non-null get), limpiar en `onDestroyView()`.
- `binding.lifecycleOwner = viewLifecycleOwner` en cada Fragment que use DataBinding.
- ViewModel: instanciar con `ViewModelProvider(this).get(...)` usando `by lazy`. **NO usar Hilt** en E2.
- LiveData: `private val _dato = MutableLiveData<T>()` + `val dato: LiveData<T> get() = _dato` (backing property).
- Guardar estado en `onSaveInstanceState()`. Restaurar en `onCreate(savedInstanceState)`.
- **NO usar** en E2: Hilt, Coroutines, Flow, StateFlow, RecyclerView, Compose, kotlinx.serialization.

### Commits y Git
- Commits con formato: `tipo(ámbito): descripción` (feat, fix, refactor, style, docs, chore).
- Ramas: `main` ← `develop` ← `feature/*`.
- **NUNCA hacer commit sin aprobación del usuario.** Antes de commitear, mostrar al usuario el mensaje de commit propuesto y los archivos que se incluirán, y esperar su OK.
- Tras crear/modificar código Android, el usuario verificará en Android Studio que compila antes de commitear.

### Registro de commits realizados
- Tras cada commit en el repo `app/`, **actualizar `ENTREGA_2.md`** en el repo de documentación: añadir el commit realizado en la tabla correspondiente, marcándolo como hecho.
- Esto aplica a **ambos miembros del equipo** (Diego y Rafael). Si el agente ayuda a hacer un commit, debe proponer también la actualización de `ENTREGA_2.md` para que quede registrado.
- El plan de commits en `ENTREGA_2.md` es orientativo; los commits reales pueden variar en fecha o mensaje. Lo importante es mantener el registro actualizado.

## Documentación del proyecto

| Ruta | Contenido |
|------|-----------|
| `00-idea-y-concepto/` | Concepto, visión, referencias |
| `01-requisitos/` | RF-01..22, RNF-01..22, CU-01..12 |
| `02-diseño/` | Arquitectura MVVM, modelo de datos |
| `03-diseño-ui/` | Mockups en `vistas/` y prompts en `prompts/` |
| `ENTREGA2/` | Normas, rúbrica y material de la Entrega 2 |
| `EL AS/` | Prototipo web del juego El As (HTML) |
| `ENTREGA_1.md` | Documento Entrega 1 (diagramas + prototipos) |
| `ENTREGA_2.md` | Plan de entrega 2 (criterios, commits, arquitectura) |
