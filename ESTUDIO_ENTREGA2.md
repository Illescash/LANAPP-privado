# PartyHub - Documento de Estudio para Entrega 2

**Asignatura:** DADM - UAM EPS 2025-2026
**Equipo:** Diego Illescas Lasa - Rafael Romero Monzon
**Fecha entrega:** 8 de abril de 2026

---

## Indice

1. [Vision general de la app](#1-vision-general-de-la-app)
2. [Configuracion del proyecto (Gradle, SDK, dependencias)](#2-configuracion-del-proyecto)
3. [Arquitectura MVVM](#3-arquitectura-mvvm)
4. [Activities: contenedores de navegacion](#4-activities)
5. [Fragments: la capa de vista](#5-fragments)
6. [ViewBinding y DataBinding](#6-viewbinding-y-databinding)
7. [ViewModel y LiveData](#7-viewmodel-y-livedata)
8. [GameEngines: logica pura Kotlin](#8-gameengines)
9. [Navegacion: Intents, Navigation Component y Safe Args](#9-navegacion)
10. [Ciclo de vida y estado](#10-ciclo-de-vida-y-estado)
11. [Escuchadores (Listeners)](#11-escuchadores)
12. [Recursos XML: layouts, colors, dimens, strings, themes](#12-recursos-xml)
13. [AndroidManifest](#13-androidmanifest)
14. [Logging con Timber](#14-logging-con-timber)
15. [Flujo completo de datos: ejemplo paso a paso](#15-flujo-completo-de-datos)
16. [Posibles preguntas de entrevista](#16-posibles-preguntas-de-entrevista)

---

## 1. Vision general de la app

PartyHub es una app Android de minijuegos para jugar con amigos de forma presencial en un solo dispositivo (modo LOCAL).

### Juegos incluidos

| Juego | Tipo | Jugadores | Mecanica |
|-------|------|-----------|----------|
| **The Mind** | Cooperativo | 2-4 | Ordenar cartas del 1-100 sin comunicarse. Se juegan por niveles, se pierden vidas por errores |
| **El As** | Competitivo | 3-6 | Baraja espanola, intercambiar cartas a ciegas, sobrevivir con 3 vidas. El Rey (12) bloquea intercambios |

### Flujo de la app

```
HubActivity
  └── HubFragment (seleccion de juego)
        ├── [Intent explicito] → TheMindActivity
        │     └── NavGraph: Config → Game → Result
        └── [Intent explicito] → ElAsActivity
              └── NavGraph: Config → Game → Result
```

---

## 2. Configuracion del proyecto

### SDK y versiones

| Configuracion | Valor | Significado |
|---------------|-------|-------------|
| `compileSdk` | 35 | API contra la que se compila (Android 15) |
| `targetSdk` | 35 | API para la que se optimiza |
| `minSdk` | 24 | Minimo soportado (Android 7.0) |
| `versionCode` | 2 | Numero interno de version (entero, para Google Play) |
| `versionName` | "2.0" | Version visible al usuario |

**Diferencias clave:**
- `compileSdk`: determina que APIs puedes usar en tu codigo
- `targetSdk`: le dice a Android que tu app esta preparada para ese nivel de API
- `minSdk`: dispositivos con API menor no pueden instalar la app

### Plugins de Gradle

```kotlin
plugins {
    alias(libs.plugins.android.application)          // Plugin Android para generar APK
    alias(libs.plugins.kotlin.android)               // Soporte Kotlin para Android
    alias(libs.plugins.androidx.navigation.safeargs)  // Genera clases type-safe para args de navegacion
}
```

### Build Features habilitadas

```kotlin
buildFeatures {
    viewBinding = true   // Genera clases de binding para acceder a vistas (reemplaza findViewById)
    dataBinding = true   // Permite expresiones @{} en XML para vincular datos del ViewModel
}
```

### Dependencias y su proposito

| Dependencia | Para que sirve |
|-------------|----------------|
| `androidx-core-ktx` | Extensiones Kotlin para APIs core de Android |
| `androidx-appcompat` | Compatibilidad hacia atras de funcionalidades Android |
| `material` | Componentes Material Design (MaterialButton, MaterialCardView) |
| `androidx-activity` | Extensiones Kotlin para Activity |
| `androidx-constraintlayout` | Sistema de layout flexible y eficiente |
| `androidx-navigation-fragment-ktx` | Navigation Component para transiciones entre fragments |
| `androidx-navigation-ui-ktx` | Helpers de UI para navegacion |
| `androidx-lifecycle-viewmodel-ktx` | ViewModel para gestionar estado de UI |
| `androidx-lifecycle-livedata-ktx` | LiveData para datos observables reactivos |
| `timber` | Libreria de logging (reemplaza Log.d) |

### Estructura del proyecto

Proyecto **single-module** con estructura:

```
app/                          ← Proyecto Android Studio
├── build.gradle.kts          ← Config raiz (plugins)
├── app/
│   ├── build.gradle.kts      ← Config modulo (SDK, dependencias, features)
│   └── src/main/
│       ├── java/com/partyhub/
│       │   ├── PartyHubApp.kt              ← Application (inicializa Timber)
│       │   ├── core/model/
│       │   │   ├── Player.kt               ← data class Player(id, name)
│       │   │   └── SpanishCard.kt          ← data class SpanishCard(number, suit)
│       │   ├── feature/hub/
│       │   │   ├── HubActivity.kt          ← Contenedor de HubFragment
│       │   │   └── HubFragment.kt          ← Pantalla principal, seleccion de juego
│       │   ├── feature/themind/
│       │   │   ├── TheMindActivity.kt      ← Contenedor NavHostFragment
│       │   │   ├── MindConfigFragment.kt   ← Configuracion (jugadores, dificultad)
│       │   │   ├── MindGameFragment.kt     ← Pantalla de juego (DataBinding)
│       │   │   ├── MindResultFragment.kt   ← Resultados + compartir
│       │   │   ├── MindViewModel.kt        ← Estado del juego (LiveData)
│       │   │   └── engine/
│       │   │       ├── MindGameEngine.kt   ← Logica pura Kotlin
│       │   │       └── MindGameState.kt    ← Data classes del estado
│       │   └── feature/elas/
│       │       ├── ElAsActivity.kt         ← Contenedor NavHostFragment
│       │       ├── AsConfigFragment.kt     ← Configuracion (jugadores)
│       │       ├── AsGameFragment.kt       ← Pantalla de juego
│       │       ├── AsResultFragment.kt     ← Resultados + compartir
│       │       ├── AsViewModel.kt          ← Estado del juego (LiveData)
│       │       └── engine/
│       │           ├── AsGameEngine.kt     ← Logica pura Kotlin
│       │           └── AsGameState.kt      ← Data classes del estado
│       └── res/
│           ├── layout/                     ← XMLs de interfaz
│           ├── navigation/                 ← NavGraphs (nav_mind, nav_as)
│           └── values/                     ← colors, dimens, strings, themes
└── gradle/
    └── libs.versions.toml                  ← Catalogo de versiones
```

---

## 3. Arquitectura MVVM

### Que es MVVM

MVVM (Model-View-ViewModel) es un patron arquitectonico que separa la app en tres capas:

| Capa | Que contiene | En PartyHub |
|------|-------------|-------------|
| **Model** | Datos y logica de negocio | GameEngines + data classes (Player, SpanishCard, GameState) |
| **View** | UI y presentacion | Fragments + XML layouts |
| **ViewModel** | Puente entre Model y View, gestiona estado | MindViewModel, AsViewModel (con LiveData) |

### Flujo de datos (unidireccional)

```
Usuario toca boton (View)
    ↓
Fragment llama a viewModel.accion()
    ↓
ViewModel delega a engine.calcularNuevoEstado()
    ↓
Engine devuelve nuevo estado inmutable
    ↓
ViewModel actualiza _gameState.value (MutableLiveData)
    ↓
LiveData notifica a los observers
    ↓
Fragment.observe() recibe el nuevo estado
    ↓
Fragment actualiza la UI
```

### Por que MVVM

1. **Separacion de responsabilidades**: cada capa tiene una funcion clara
2. **Testabilidad**: el Engine se testea sin Android, el ViewModel sin UI
3. **Supervivencia a rotacion**: el ViewModel sobrevive configuration changes
4. **Reactividad**: LiveData notifica cambios automaticamente

---

## 4. Activities

Las Activities en PartyHub son **contenedores minimos**. La logica real vive en los Fragments.

### HubActivity

```kotlin
class HubActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_hub)
    }
}
```

- Extiende `AppCompatActivity` (compatibilidad hacia atras)
- Solo llama a `setContentView()` con un layout que contiene un `FragmentContainerView`
- El layout carga `HubFragment` directamente por nombre de clase

### TheMindActivity y ElAsActivity

```kotlin
class TheMindActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_the_mind)
    }
}
```

- Mismo patron minimo
- Su layout contiene un `NavHostFragment` que gestiona la navegacion entre fragments
- `app:navGraph="@navigation/nav_mind"` apunta al grafo de navegacion
- `app:defaultNavHost="true"` permite que el boton "atras" navegue por el grafo

**Por que Activities separadas por juego?**
- Cada juego tiene su propio NavGraph y back stack independiente
- Limpia separacion de estado entre juegos
- Al hacer `finish()` en el resultado, se vuelve al Hub directamente

---

## 5. Fragments

### Patron comun de binding en todos los Fragments

```kotlin
class MiFragment : Fragment() {
    // 1. Backing field nullable
    private var _binding: FragmentMiBinding? = null
    // 2. Getter non-null (solo usar entre onCreateView y onDestroyView)
    private val binding get() = _binding!!

    override fun onCreateView(
        inflater: LayoutInflater, container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View {
        // 3. Inflar el binding
        _binding = FragmentMiBinding.inflate(inflater, container, false)
        return binding.root
    }

    override fun onDestroyView() {
        super.onDestroyView()
        // 4. CRITICO: limpiar referencia para evitar memory leaks
        _binding = null
    }
}
```

**Por que este patron?**
- `_binding` es nullable porque la vista no existe antes de `onCreateView` ni despues de `onDestroyView`
- `binding` con `!!` es seguro SOLO entre esos dos callbacks
- Limpiar en `onDestroyView()` evita que el Fragment retenga referencias a vistas destruidas (memory leak)

### HubFragment — Pantalla principal

**Tipo de binding:** ViewBinding (sin DataBinding, contenido estatico)
**ViewModel:** No tiene (solo navega)

```kotlin
override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
    super.onViewCreated(view, savedInstanceState)

    // Intent EXPLICITO: sabemos exactamente que Activity abrir
    binding.btnTheMind.setOnClickListener {
        val intent = Intent(requireContext(), TheMindActivity::class.java)
        startActivity(intent)
    }
    binding.btnElAs.setOnClickListener {
        val intent = Intent(requireContext(), ElAsActivity::class.java)
        startActivity(intent)
    }
}
```

- Usa `requireContext()` en vez de `context` (lanza excepcion si Fragment no esta attached, mas seguro)
- **Intent explicito**: especifica la clase destino directamente

### MindConfigFragment — Configuracion The Mind

**Tipo de binding:** DataBinding (declara variable ViewModel en XML)
**ViewModel:** Si (MindViewModel)
**Estado guardado:** Si (onSaveInstanceState para RadioButtons)

```kotlin
private val viewModel: MindViewModel by lazy {
    ViewModelProvider(this).get(MindViewModel::class.java)
}

override fun onCreateView(...): View {
    _binding = FragmentMindConfigBinding.inflate(inflater, container, false)
    binding.viewModel = viewModel                    // Inyectar VM en binding
    binding.lifecycleOwner = viewLifecycleOwner      // Habilitar observacion LiveData en XML
    return binding.root
}
```

**Restauracion de estado:**
```kotlin
override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
    super.onViewCreated(view, savedInstanceState)

    // Restaurar selecciones si venimos de rotacion
    savedInstanceState?.let { state ->
        binding.rgPlayers.check(state.getInt(KEY_PLAYERS, R.id.rbPlayers2))
        binding.rgDifficulty.check(state.getInt(KEY_DIFFICULTY, R.id.rbEasy))
    }

    // Navegar con Safe Args al pulsar "Empezar"
    binding.btnStart.setOnClickListener {
        val numPlayers = when (binding.rgPlayers.checkedRadioButtonId) {
            R.id.rbPlayers2 -> 2
            R.id.rbPlayers3 -> 3
            R.id.rbPlayers4 -> 4
            else -> 2
        }
        val difficulty = when (binding.rgDifficulty.checkedRadioButtonId) {
            R.id.rbEasy -> "EASY"
            R.id.rbNormal -> "NORMAL"
            R.id.rbHard -> "HARD"
            else -> "NORMAL"
        }
        // Safe Args: genera clase Directions con tipos seguros
        val action = MindConfigFragmentDirections
            .actionMindConfigFragmentToMindGameFragment(numPlayers, difficulty)
        findNavController().navigate(action)
    }
}

override fun onSaveInstanceState(outState: Bundle) {
    super.onSaveInstanceState(outState)
    _binding?.let { b ->
        outState.putInt(KEY_PLAYERS, b.rgPlayers.checkedRadioButtonId)
        outState.putInt(KEY_DIFFICULTY, b.rgDifficulty.checkedRadioButtonId)
    }
}
```

### MindGameFragment — Pantalla de juego The Mind

**Tipo de binding:** DataBinding (expresiones `@{}` en XML)
**ViewModel:** Si (MindViewModel, con LiveData observado)
**Creacion dinamica de vistas:** Si (botones de jugador)

```kotlin
private val args: MindGameFragmentArgs by navArgs()  // Recibir args tipados

override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
    super.onViewCreated(view, savedInstanceState)

    // Solo inicializar el juego la PRIMERA vez (no en rotacion)
    if (viewModel.gameState.value == null) {
        val playerNames = (1..args.numPlayers).map { "Jugador $it" }
        val initialLives = when (args.difficulty) {
            "EASY" -> 4; "HARD" -> 2; else -> 3
        }
        viewModel.startGame(playerNames, initialLives)
    }
    setupObservers()
    setupClickListeners()
}
```

**Observacion de LiveData:**
```kotlin
private fun setupObservers() {
    viewModel.gameState.observe(viewLifecycleOwner) { state ->
        // Actualizar visibilidad segun estado
        binding.tvStatus.isVisible = state.status == MindStatus.REVEALING
        binding.btnResolve.isVisible = state.status == MindStatus.REVEALING
        binding.btnNextLevel.isVisible = state.status == MindStatus.LEVEL_COMPLETE

        // Navegar a resultados cuando termina
        if (state.status == MindStatus.GAME_OVER || state.status == MindStatus.VICTORY) {
            val action = MindGameFragmentDirections
                .actionMindGameFragmentToMindResultFragment(
                    levelReached = state.level,
                    isVictory = state.status == MindStatus.VICTORY
                )
            findNavController().navigate(action)
        }

        // Actualizar botones de jugador dinamicamente
        updatePlayerActions(state.players.map { it.id }, state.playerHands)
    }
}
```

**Creacion dinamica de vistas (sin RecyclerView):**
```kotlin
private fun updatePlayerActions(playerIds: List<String>, playerHands: Map<String, List<Int>>) {
    binding.playerActionsContainer.removeAllViews()  // Limpiar vistas anteriores

    playerIds.forEach { playerId ->
        val hand = playerHands[playerId] ?: emptyList()
        if (hand.isNotEmpty()) {
            // Inflar layout de item
            val playerView = layoutInflater.inflate(
                R.layout.item_mind_player_action,
                binding.playerActionsContainer, false
            )
            // Configurar vistas del item
            playerView.findViewById<TextView>(R.id.tvPlayerName).text =
                getString(R.string.mind_label_player_hand, playerId)
            playerView.findViewById<Button>(R.id.btnPlayCard).apply {
                text = getString(R.string.mind_btn_play_card, hand.first())
                setOnClickListener { viewModel.playCard(playerId) }
                isEnabled = viewModel.gameState.value?.status == MindStatus.PLAYING
            }
            // Anadir al contenedor
            binding.playerActionsContainer.addView(playerView)
        }
    }
}
```

### MindResultFragment — Resultados The Mind

**Tipo de binding:** ViewBinding (contenido estatico, datos via Safe Args)
**ViewModel:** No (solo muestra resultados)
**Intent implicito:** Si (compartir resultado)

```kotlin
private val args: MindResultFragmentArgs by navArgs()

override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
    super.onViewCreated(view, savedInstanceState)

    Timber.d("The Mind: pantalla de resultados — victoria=${args.isVictory}, nivel=${args.levelReached}")

    // Mostrar resultado
    binding.tvResultStatus.text = if (args.isVictory) {
        getString(R.string.mind_result_victory)
    } else {
        getString(R.string.mind_result_game_over)
    }
    binding.tvLevelReached.text = getString(R.string.mind_result_level, args.levelReached)

    // INTENT IMPLICITO: compartir resultado
    binding.btnShare.setOnClickListener {
        val text = getString(R.string.mind_share_text, args.levelReached)
        val intent = Intent(Intent.ACTION_SEND).apply {
            type = "text/plain"
            putExtra(Intent.EXTRA_TEXT, text)
        }
        startActivity(Intent.createChooser(intent, null))  // Chooser para elegir app
    }

    // Volver al Hub cerrando la Activity
    binding.btnBackToHub.setOnClickListener {
        activity?.finish()
    }
}
```

### AsConfigFragment, AsGameFragment, AsResultFragment

Siguen los mismos patrones que los de The Mind:
- **AsConfigFragment**: ViewBinding, sin ViewModel, onSaveInstanceState para rgPlayers, navega con Safe Args (numPlayers)
- **AsGameFragment**: ViewBinding, AsViewModel con LiveData, observa gameState, botones stay/swap/resolve/next
- **AsResultFragment**: ViewBinding, sin ViewModel, recibe winnerName por Safe Args, intent implicito compartir

### Resumen de Fragments

| Fragment | Binding | ViewModel | DataBinding | SaveState | Safe Args |
|----------|---------|-----------|-------------|-----------|-----------|
| HubFragment | ViewBinding | No | No | No | No |
| MindConfigFragment | DataBinding | Si | Si | Si (players, difficulty) | Envia: numPlayers, difficulty |
| MindGameFragment | DataBinding | Si | Si (@{}) | No (ViewModel sobrevive) | Recibe: numPlayers, difficulty |
| MindResultFragment | ViewBinding | No | No | No | Recibe: levelReached, isVictory |
| AsConfigFragment | ViewBinding | No | No | Si (players) | Envia: numPlayers |
| AsGameFragment | ViewBinding | Si | No | No (ViewModel sobrevive) | Recibe: numPlayers |
| AsResultFragment | ViewBinding | No | No | No | Recibe: winnerName |

---

## 6. ViewBinding y DataBinding

### ViewBinding

**Que es:** Sistema que genera una clase de binding para cada layout XML. Reemplaza `findViewById()`.

**Ventajas sobre findViewById:**
- **Type-safe**: cada vista tiene el tipo correcto (TextView, Button, etc.)
- **Null-safe**: si una vista no existe en el layout, no compila
- **Sin casting**: no hay que hacer `as TextView`

**Como se usa en Activities:**
```kotlin
class HubActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_hub)  // Simple, sin binding
    }
}
```
(Nota: nuestras Activities son tan simples que no necesitan binding)

**Como se usa en Fragments:**
```kotlin
// Inflar
_binding = FragmentHubBinding.inflate(inflater, container, false)
// Acceder a vistas
binding.btnTheMind.setOnClickListener { ... }
// Limpiar
_binding = null  // en onDestroyView
```

### DataBinding

**Que es:** Extension de ViewBinding que permite escribir expresiones en el XML que se vinculan a datos del ViewModel.

**Diferencia con ViewBinding:**

| Caracteristica | ViewBinding | DataBinding |
|----------------|-------------|-------------|
| Genera clase binding | Si | Si |
| Reemplaza findViewById | Si | Si |
| Expresiones en XML `@{}` | No | Si |
| Tag `<layout>` requerido | No | Si |
| Bloque `<data>` con variables | No | Si |
| Observacion automatica LiveData | No | Si (con lifecycleOwner) |

**Como se declara en XML (DataBinding):**
```xml
<layout xmlns:android="...">
    <data>
        <variable
            name="viewModel"
            type="com.partyhub.feature.themind.MindViewModel" />
    </data>

    <ConstraintLayout ...>
        <!-- Expresion @{} que se actualiza automaticamente -->
        <TextView
            android:text="@{String.format(@string/mind_label_level, viewModel.gameState.level)}" />
    </ConstraintLayout>
</layout>
```

**Como se configura en el Fragment:**
```kotlin
_binding = FragmentMindConfigBinding.inflate(inflater, container, false)
binding.viewModel = viewModel                // Asignar el ViewModel al XML
binding.lifecycleOwner = viewLifecycleOwner  // CRITICO: sin esto LiveData no se observa
```

**Expresiones DataBinding usadas en PartyHub:**
```xml
<!-- Nivel actual (formateado con string resource) -->
android:text="@{String.format(@string/mind_label_level, viewModel.gameState.level)}"

<!-- Vidas restantes -->
android:text="@{String.format(@string/mind_label_lives, viewModel.gameState.lives)}"

<!-- Carta en la pila central (con operador ternario) -->
android:text="@{viewModel.gameState.playedCards.empty ?
    `?` :
    String.valueOf(viewModel.gameState.playedCards[viewModel.gameState.playedCards.size() - 1].number)}"
```

**Por que usar DataBinding en los GameFragments pero no en todos los Fragments?**
- Los GameFragments tienen datos reactivos que cambian constantemente (nivel, vidas, cartas)
- DataBinding actualiza la UI automaticamente cuando LiveData cambia
- Los ConfigFragments y ResultFragments son mas estaticos, ViewBinding es suficiente

---

## 7. ViewModel y LiveData

### ViewModel

**Que es:** Componente de Android Architecture Components que almacena y gestiona datos de UI de forma consciente del ciclo de vida.

**Caracteristica clave:** **Sobrevive a configuration changes** (rotacion de pantalla). Cuando el dispositivo rota, la Activity/Fragment se destruye y recrea, pero el ViewModel persiste en memoria.

**Como se crea:**
```kotlin
// En el Fragment, con by lazy (se crea solo cuando se accede por primera vez)
private val viewModel: MindViewModel by lazy {
    ViewModelProvider(this).get(MindViewModel::class.java)
}
```

- `ViewModelProvider(this)`: el `this` es el Fragment, determina el scope del ViewModel
- `.get(MindViewModel::class.java)`: obtiene instancia existente o crea una nueva
- `by lazy`: no se ejecuta hasta que se accede a `viewModel` por primera vez

**Ciclo de vida del ViewModel:**
```
Fragment creado por primera vez
    → ViewModel creado (by lazy, primera vez que se accede)
    → gameState.value = null (no inicializado)

Dispositivo rota
    → Fragment DESTRUIDO
    → Fragment RECREADO
    → ViewModel NO se destruye (ViewModelProvider devuelve la misma instancia)
    → gameState.value mantiene el estado del juego

Usuario pulsa "atras" (navega fuera)
    → Fragment destruido definitivamente
    → ViewModel destruido (garbage collected)
```

### LiveData y el patron backing property

```kotlin
class MindViewModel : ViewModel() {
    private val engine = MindGameEngine()

    // PRIVADO: MutableLiveData (el ViewModel puede escribir)
    private val _gameState = MutableLiveData<MindGameState>()

    // PUBLICO: LiveData (los Fragments solo pueden leer/observar)
    val gameState: LiveData<MindGameState> get() = _gameState
}
```

**Por que este patron?**
- **Encapsulacion**: solo el ViewModel puede modificar el estado
- **Seguridad**: los Fragments no pueden corromper el estado accidentalmente
- **Flujo unidireccional**: cambios solo a traves de los metodos publicos del ViewModel

**MutableLiveData vs LiveData:**

| | MutableLiveData | LiveData |
|-|-----------------|----------|
| Leer valor | `.value` | `.value` |
| Escribir valor | `.value = nuevoValor` | NO se puede |
| Observar cambios | `.observe(...)` | `.observe(...)` |
| Quien lo usa | ViewModel (interno) | Fragment (externo) |

### Como se observa LiveData desde un Fragment

```kotlin
viewModel.gameState.observe(viewLifecycleOwner) { state ->
    // Este lambda se ejecuta CADA VEZ que gameState cambia
    binding.tvLevel.text = "Mision ${state.level}"
    binding.tvLives.text = "Vidas: ${state.lives}"
}
```

**Por que `viewLifecycleOwner` y no `this`?**
- Un Fragment tiene DOS ciclos de vida: el del Fragment y el de su View
- `viewLifecycleOwner` esta ligado al ciclo de vida de la **View**
- Si usaras `this` (el Fragment), el observer podria recibir callbacks cuando la View ya no existe
- `viewLifecycleOwner` se autodestruye en `onDestroyView`, evitando crashes

### Metodos del MindViewModel

```kotlin
fun startGame(playerNames: List<String>, lives: Int = 3) {
    _gameState.value = engine.newGame(playerNames, lives)
}

fun playCard(playerId: String) {
    val current = _gameState.value ?: return  // Null safety con early return
    _gameState.value = engine.playCard(current, playerId)
}

fun resolveLevel() {
    val current = _gameState.value ?: return
    _gameState.value = engine.resolveLevel(current)
}

fun nextLevel() {
    val current = _gameState.value ?: return
    _gameState.value = engine.startNextLevel(current)
}
```

**Patron comun en todos los metodos:**
1. Obtener estado actual de `_gameState.value` (con `?: return` si es null)
2. Delegar al engine la logica de negocio
3. El engine devuelve un NUEVO estado (inmutable)
4. Asignar el nuevo estado a `_gameState.value`
5. LiveData notifica automaticamente a los observers

### Metodos del AsViewModel

```kotlin
fun startGame(playerCount: Int) {
    _gameState.value = engine.startNewGame(playerCount)
}

fun swap() {
    val current = _gameState.value ?: return
    _gameState.value = engine.swap(current)
}

fun stay() {
    val current = _gameState.value ?: return
    _gameState.value = engine.stay(current)
}

fun resolveRound() {
    val current = _gameState.value ?: return
    _gameState.value = engine.resolveRound(current)
}

fun nextRound() {
    val current = _gameState.value ?: return
    _gameState.value = engine.nextRound(current)
}
```

---

## 8. GameEngines

### Principio fundamental: CERO imports de Android

Los GameEngines son **Kotlin puro**. No importan nada de `android.*`. Esto permite:
- Testeo unitario sin emulador ni framework Android
- Reutilizacion en otros contextos (modo LAN en E3, tests, etc.)
- Separacion total entre logica de negocio y UI

### MindGameEngine — Data classes

```kotlin
data class MindGameState(
    val level: Int,                          // Nivel actual (1 a maxLevel)
    val lives: Int,                          // Vidas restantes
    val players: List<Player>,               // Lista de jugadores
    val playerHands: Map<String, List<Int>>, // playerId -> cartas ordenadas
    val playedCards: List<PlayedCard>,       // Historial de cartas jugadas
    val pendingCards: List<Int>,             // Cartas que faltan por jugar (ordenadas)
    val status: MindStatus                   // Fase actual del juego
)

data class PlayedCard(
    val number: Int,        // Valor de la carta (1-100)
    val playerId: String,   // Quien la jugo
    val wasCorrect: Boolean // Era la siguiente en orden?
)

enum class MindStatus {
    PLAYING,         // Jugadores estan jugando cartas
    REVEALING,       // Todas jugadas, mostrando resultados
    LEVEL_COMPLETE,  // Nivel superado
    GAME_OVER,       // Sin vidas
    VICTORY          // Todos los niveles completados
}
```

### MindGameEngine — Metodos

**`newGame(playerNames, lives)`**: Crea estado inicial. Reparte 1 carta por jugador (nivel 1).

**`playCard(state, playerId)`**: Juega la carta mas baja del jugador. Compara con la carta pendiente mas baja. Si es correcta, se elimina de pendientes. Si no, se eliminan tambien las cartas menores. Cuando todas las manos estan vacias, cambia a REVEALING.

**`resolveLevel(state)`**: Cuenta errores en las cartas jugadas. Resta vidas. Si vidas <= 0, GAME_OVER. Si no, LEVEL_COMPLETE.

**`startNextLevel(state)`**: Incrementa nivel. Si supera el maximo (12 - numPlayers + 1), VICTORY. Si no, reparte nuevas cartas (nivel N = N cartas por jugador).

**`dealCards(players, level)`**: Baraja cartas 1-100, reparte `level` cartas a cada jugador, ordena cada mano.

### AsGameEngine — Data classes

```kotlin
data class AsGameState(
    val players: List<AsPlayer>,      // Jugadores con su carta actual
    val deck: List<SpanishCard>,      // Baraja restante
    val currentPlayerIndex: Int,       // Turno actual
    val status: AsStatus,              // Fase del juego
    val lastAction: String? = null     // Mensaje de accion bloqueada
)

data class AsPlayer(
    val player: Player,                // Datos base (id, name)
    val hand: SpanishCard?,            // Carta actual (null si eliminado)
    val lives: Int = 3,                // Vidas
    val isOut: Boolean = false         // Eliminado?
)

enum class AsStatus {
    WAITING_ACTION,  // Esperando swap/stay
    REVEALING,       // Mostrando cartas, determinando perdedor
    ROUND_OVER,      // Vidas deducidas, preparar siguiente ronda
    GAME_OVER        // Solo queda un jugador
}
```

### AsGameEngine — Metodos

**`generateDeck()`**: Genera baraja espanola (40 cartas: 1-7, 10-12 x 4 palos). Baraja y devuelve.

**`startNewGame(playerCount)`**: Genera baraja, reparte 1 carta a cada jugador, estado inicial.

**`swap(state)`**: Jugador actual intercambia carta. Si es el ultimo jugador, intercambia con la baraja. Si el siguiente tiene un Rey (12), bloquea el intercambio. Avanza turno.

**`stay(state)`**: Jugador se queda con su carta. Solo avanza turno.

**`resolveRound(state)`**: Busca el valor mas bajo entre las cartas. El jugador con la carta mas baja pierde una vida. Si queda <= 1 jugador vivo, GAME_OVER.

**`nextRound(state)`**: Genera nueva baraja, reparte cartas a jugadores vivos, reinicia turnos.

### Modelo core compartido

```kotlin
// core/model/Player.kt
data class Player(
    val id: String,   // "0", "1", etc.
    val name: String  // "Jugador 1", etc.
)

// core/model/SpanishCard.kt
data class SpanishCard(
    val number: Int,
    val suit: Suit
) {
    enum class Suit { OROS, COPAS, ESPADAS, BASTOS }
    val value: Int get() = number
}
```

### Inmutabilidad del estado

Todos los cambios de estado crean NUEVOS objetos. Nunca se muta el estado existente:

```kotlin
// En el engine, se usa .copy() para crear nuevo estado
return state.copy(
    playerHands = newHands,      // Manos actualizadas
    playedCards = newPlayedCards, // Nueva carta anadida
    pendingCards = newPending,   // Carta eliminada
    status = newStatus           // Nuevo estado
)
```

**Ventajas de la inmutabilidad:**
- Facil de razonar (el estado anterior no cambia)
- Thread-safe sin locks
- Permite undo/redo (guardando estados anteriores)
- Sin efectos secundarios inesperados

---

## 9. Navegacion

### Intents explicitos (Hub → Juegos)

```kotlin
// HubFragment: abrir una Activity especifica
val intent = Intent(requireContext(), TheMindActivity::class.java)
startActivity(intent)
```

**Intent explicito**: especifica exactamente que componente (Activity) abrir. Se usa cuando conocemos el destino.

### Intents implicitos (Compartir resultado)

```kotlin
// MindResultFragment: compartir resultado con cualquier app
val intent = Intent(Intent.ACTION_SEND).apply {
    type = "text/plain"
    putExtra(Intent.EXTRA_TEXT, "Hemos llegado al nivel 5 en The Mind!")
}
startActivity(Intent.createChooser(intent, null))
```

**Intent implicito**: no especifica componente, sino una ACCION. Android busca todas las apps que pueden manejar esa accion (WhatsApp, Email, Twitter...) y muestra un chooser.

**Diferencias:**

| | Intent explicito | Intent implicito |
|-|-----------------|-----------------|
| Destino | Clase especifica | Accion generica |
| Uso | Navegacion interna | Interaccion con otras apps |
| Ejemplo | `Intent(ctx, TheMindActivity::class.java)` | `Intent(Intent.ACTION_SEND)` |
| Chooser | No | Si (`createChooser`) |

### Navigation Component (entre Fragments)

**Que es:** Libreria Jetpack que gestiona la navegacion entre Fragments dentro de una Activity.

**Componentes:**
- **NavGraph** (XML): define los fragments y las acciones (transiciones) entre ellos
- **NavHostFragment**: contenedor en el layout de la Activity que muestra los fragments
- **NavController**: objeto que ejecuta la navegacion
- **Safe Args**: plugin que genera clases type-safe para pasar argumentos

### NavGraph de The Mind (nav_mind.xml)

```xml
<navigation app:startDestination="@id/mindConfigFragment">

    <fragment android:id="@+id/mindConfigFragment"
              android:name="...MindConfigFragment">
        <action android:id="@+id/action_mindConfigFragment_to_mindGameFragment"
                app:destination="@id/mindGameFragment" />
    </fragment>

    <fragment android:id="@+id/mindGameFragment"
              android:name="...MindGameFragment">
        <argument android:name="numPlayers" app:argType="integer" />
        <argument android:name="difficulty" app:argType="string" />
        <action android:id="@+id/action_mindGameFragment_to_mindResultFragment"
                app:destination="@id/mindResultFragment" />
    </fragment>

    <fragment android:id="@+id/mindResultFragment"
              android:name="...MindResultFragment">
        <argument android:name="levelReached" app:argType="integer" />
        <argument android:name="isVictory" app:argType="boolean" />
    </fragment>
</navigation>
```

**Flujo:**
```
MindConfigFragment → (numPlayers: Int, difficulty: String) → MindGameFragment
MindGameFragment → (levelReached: Int, isVictory: Boolean) → MindResultFragment
```

### NavGraph de El As (nav_as.xml)

```
AsConfigFragment → (numPlayers: Int) → AsGameFragment
AsGameFragment → (winnerName: String) → AsResultFragment
```

### Safe Args — Enviar argumentos

```kotlin
// En MindConfigFragment: crear accion con argumentos tipados
val action = MindConfigFragmentDirections
    .actionMindConfigFragmentToMindGameFragment(numPlayers, difficulty)
findNavController().navigate(action)
```

`MindConfigFragmentDirections` es una clase GENERADA por el plugin Safe Args a partir del NavGraph. Garantiza que:
- `numPlayers` es `Int` (no String, no null)
- `difficulty` es `String` (no Int, no null)
- El nombre de la accion existe en el NavGraph

### Safe Args — Recibir argumentos

```kotlin
// En MindGameFragment: recibir argumentos tipados
private val args: MindGameFragmentArgs by navArgs()

// Uso:
val numPlayers = args.numPlayers    // Int, tipo seguro
val difficulty = args.difficulty     // String, tipo seguro
```

`navArgs()` es un delegate que extrae los argumentos del Bundle automaticamente.

---

## 10. Ciclo de vida y estado

### Ciclo de vida de una Activity

```
[No existe] → onCreate() → onStart() → onResume() → [ACTIVA]
                                                        ↓
[DESTRUIDA] ← onDestroy() ← onStop() ← onPause() ← [PAUSADA]
```

| Callback | Cuando se llama | Que hacer |
|----------|-----------------|-----------|
| `onCreate()` | Activity se crea (o recrea tras rotacion) | Inicializar UI, restaurar estado |
| `onStart()` | Activity visible (pero no en primer plano) | Registrar listeners |
| `onResume()` | Activity en primer plano, interactuable | Iniciar animaciones |
| `onPause()` | Pierde foco (dialogo encima, otra app) | Pausar operaciones costosas |
| `onStop()` | Ya no visible | Liberar recursos |
| `onDestroy()` | Activity se destruye (back, finish(), rotacion) | Limpieza final |

### Configuration changes (rotacion)

Cuando el dispositivo rota:
1. Activity recibe `onPause()` → `onStop()` → `onSaveInstanceState()` → `onDestroy()`
2. Activity se RECREA: `onCreate(savedInstanceState)` → `onStart()` → `onResume()`

**Lo que sobrevive:**
- **ViewModel**: SI (ViewModelProvider lo mantiene en memoria)
- **savedInstanceState (Bundle)**: SI (el sistema lo guarda y lo pasa al nuevo onCreate)
- **Variables locales de la Activity/Fragment**: NO (se pierden)

### onSaveInstanceState — Guardar estado

```kotlin
override fun onSaveInstanceState(outState: Bundle) {
    super.onSaveInstanceState(outState)
    _binding?.let { b ->
        outState.putInt(KEY_PLAYERS, b.rgPlayers.checkedRadioButtonId)
        outState.putInt(KEY_DIFFICULTY, b.rgDifficulty.checkedRadioButtonId)
    }
}
```

- Se llama ANTES de que la Activity/Fragment se destruya
- Se guarda informacion en un `Bundle` (pares clave-valor)
- El Bundle se pasa a `onCreate(savedInstanceState)` cuando se recrea
- Usamos `_binding?.let` por seguridad (puede ser null si onDestroyView ya se llamo)

### Restaurar estado

```kotlin
override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
    super.onViewCreated(view, savedInstanceState)
    savedInstanceState?.let { state ->
        binding.rgPlayers.check(state.getInt(KEY_PLAYERS, R.id.rbPlayers2))
    }
}
```

- `savedInstanceState` es null la primera vez, no-null si viene de rotacion
- `?.let` ejecuta el bloque solo si no es null
- `getInt(key, default)` obtiene el valor guardado o el default

### Cuando usar onSaveInstanceState vs ViewModel

| | onSaveInstanceState | ViewModel |
|-|---------------------|-----------|
| Sobrevive rotacion | Si | Si |
| Sobrevive proceso kill | Si (Bundle persiste) | No (se pierde) |
| Tipo de datos | Primitivos, Parcelable | Cualquier objeto |
| Tamano maximo | ~1MB | Sin limite (memoria) |
| Uso en PartyHub | RadioButton selections (config) | Estado completo del juego |

**En PartyHub:**
- **ConfigFragments**: usan `onSaveInstanceState` para las selecciones de RadioButton
- **GameFragments**: NO lo usan porque el ViewModel ya sobrevive la rotacion con todo el estado del juego

---

## 11. Escuchadores

### OnClickListener

```kotlin
// Lambda (forma concisa)
binding.btnStart.setOnClickListener {
    // accion al pulsar
}

// Equivalente con objeto anonimo
binding.btnStart.setOnClickListener(object : View.OnClickListener {
    override fun onClick(v: View) {
        // accion al pulsar
    }
})
```

### Escuchadores en PartyHub

| Fragment | Boton | Accion |
|----------|-------|--------|
| HubFragment | btnTheMind | Intent explicito → TheMindActivity |
| HubFragment | btnElAs | Intent explicito → ElAsActivity |
| MindConfigFragment | btnStart | Navegar con Safe Args → MindGameFragment |
| MindGameFragment | btnPlayCard (dinamico) | viewModel.playCard(playerId) |
| MindGameFragment | btnResolve | viewModel.resolveLevel() |
| MindGameFragment | btnNextLevel | viewModel.nextLevel() |
| MindResultFragment | btnShare | Intent implicito ACTION_SEND |
| MindResultFragment | btnBackToHub | activity?.finish() |
| AsConfigFragment | btnStart | Navegar con Safe Args → AsGameFragment |
| AsGameFragment | btnStay | viewModel.stay() |
| AsGameFragment | btnSwap | viewModel.swap() |
| AsGameFragment | btnResolveRound | viewModel.resolveRound() |
| AsGameFragment | btnNextRound | viewModel.nextRound() |
| AsResultFragment | btnShare | Intent implicito ACTION_SEND |
| AsResultFragment | btnBackToHub | activity?.finish() |

---

## 12. Recursos XML

### Layouts — Tipos usados

| Layout | Tipo raiz | DataBinding | Proposito |
|--------|-----------|-------------|-----------|
| activity_hub | FragmentContainerView | No | Contenedor de HubFragment |
| activity_the_mind | FragmentContainerView (NavHost) | No | Contenedor de NavGraph Mind |
| activity_el_as | FragmentContainerView (NavHost) | No | Contenedor de NavGraph As |
| fragment_hub | ConstraintLayout | No | Seleccion de juego (MaterialCardView) |
| fragment_mind_config | ConstraintLayout | Si | RadioGroups + MaterialButton |
| fragment_mind_game | ConstraintLayout | Si (@{}) | Juego activo con expresiones |
| fragment_mind_result | ConstraintLayout | No | Resultados + compartir |
| fragment_as_config | ConstraintLayout | Si | RadioGroup + Button |
| fragment_as_game | ConstraintLayout | No | Juego activo |
| fragment_as_result | ConstraintLayout | No | Resultados + compartir |
| item_mind_player_action | LinearLayout | No | Item dinamico (jugador + boton) |

### colors.xml — Paleta de colores

```xml
<!-- Colores Material base -->
purple_500 (#FF8C30F5)    — Color primario (neon violeta)
purple_700 (#FF3700B3)    — Variante oscura del primario
teal_200   (#FF03DAC5)    — Color secundario (neon teal)
black      (#FF121212)    — Fondo oscuro (OLED-friendly)
white      (#FFFFFFFF)    — Texto principal
gray_text  (#FFBDBDBD)    — Texto secundario

<!-- Colores personalizados PartyHub -->
party_surface    (#FF1E1E1E) — Fondo de tarjetas/superficies
party_card_mind  (#FF4A148C) — Card de The Mind (morado profundo)
party_card_as    (#FFC62828) — Card de El As (rojo intenso)

<!-- Colores de texto -->
text_light_gray  (#FFE0E0E0) — Descripciones
text_secondary   (#FFAAAAAA) — Labels terciarios
text_lives       (#FFFF5252) — Contador de vidas (rojo alerta)
text_status_gold (#FFFFD700) — Mensajes de estado (dorado)

<!-- Colores de botones -->
card_background  (#FF2C2C2C) — Fondo de carta
btn_primary      (#FF3D5AFE) — Boton principal (azul indigo)
btn_share        (#FF00897B) — Boton compartir (teal)
btn_as_accent    (#FFFF9100) — Boton El As (naranja)
```

### dimens.xml — Sistema de espaciado

```xml
<!-- Margenes (base 8dp) -->
margin_tiny     4dp      <!-- Espaciado minimo -->
margin_small    8dp      <!-- 1x base -->
margin_medium   12dp     <!-- 1.5x base -->
margin_normal   16dp     <!-- 2x base (mas usado) -->
margin_large    24dp     <!-- 3x base -->
margin_xlarge   32dp     <!-- 4x base -->
margin_xxlarge  48dp     <!-- 6x base -->
margin_huge     64dp     <!-- 8x base -->

<!-- Tamanos de texto -->
text_card_desc       14sp   <!-- Descripciones pequenas -->
text_subtitle        16sp   <!-- Subtitulos -->
text_body            18sp   <!-- Texto normal -->
text_body_large      20sp   <!-- Texto grande -->
text_card_title      24sp   <!-- Titulos de carta -->
text_title_large     32sp   <!-- Titulos de pantalla -->
text_result          42sp   <!-- Victoria/Derrota -->
text_card_number     48sp   <!-- Numero en carta (grande) -->

<!-- Dimensiones de cartas -->
card_width_mind  120dp / card_height_mind  160dp  <!-- Carta The Mind -->
card_width_as    150dp / card_height_as    220dp  <!-- Carta El As (mas grande) -->
card_corner_radius  16dp  <!-- Esquinas redondeadas -->
card_elevation       8dp  <!-- Sombra -->
```

### strings.xml — Textos externalizados

Todos los textos visibles estan en `strings.xml`, no hardcodeados en los layouts. Esto permite:
- Cambiar textos sin tocar layouts
- Futura localizacion (traducir a otros idiomas)
- Usar placeholders: `%d` (enteros), `%s` (strings), `%1$s` (posicional)

Ejemplos:
```xml
<string name="mind_label_level">Mision %d</string>
<string name="mind_share_text">Hemos llegado al nivel %1$d en The Mind de PartyHub!</string>
<string name="as_result_winner">%s gana!</string>
```

### themes.xml

```xml
<style name="Theme.PartyHub" parent="Theme.MaterialComponents.DayNight.NoActionBar">
    <item name="colorPrimary">@color/purple_500</item>
    <item name="colorPrimaryVariant">@color/purple_700</item>
    <item name="colorOnPrimary">@color/white</item>
    <item name="colorSecondary">@color/teal_200</item>
    <item name="colorSecondaryVariant">@color/teal_700</item>
    <item name="colorOnSecondary">@color/black</item>
</style>
```

- **NoActionBar**: sin barra de accion (cada pantalla gestiona su propia UI)
- **DayNight**: soporte para tema claro/oscuro del sistema
- **MaterialComponents**: componentes modernos (MaterialButton, MaterialCardView)

---

## 13. AndroidManifest

```xml
<application
    android:name=".PartyHubApp"
    android:icon="@mipmap/ic_launcher"
    android:label="@string/app_name"
    android:theme="@style/Theme.PartyHub"
    android:supportsRtl="true">

    <!-- Punto de entrada: EXPORTED para que el launcher la abra -->
    <activity android:name=".feature.hub.HubActivity"
              android:exported="true">
        <intent-filter>
            <action android:name="android.intent.action.MAIN" />
            <category android:name="android.intent.category.LAUNCHER" />
        </intent-filter>
    </activity>

    <!-- Juegos: NO exported (solo uso interno) -->
    <activity android:name=".feature.themind.TheMindActivity"
              android:exported="false" />
    <activity android:name=".feature.elas.ElAsActivity"
              android:exported="false" />
</application>
```

**`android:exported`:**
- `true`: otras apps/el sistema pueden lanzar esta Activity (necesario para LAUNCHER)
- `false`: solo accesible internamente (nuestros Intents explicitos)
- Obligatorio desde Android 12 (API 31)

**Intent-filter MAIN + LAUNCHER:**
- `MAIN`: es el punto de entrada de la app
- `LAUNCHER`: aparece en el cajon de apps del dispositivo
- Solo UNA activity debe tener este intent-filter

---

## 14. Logging con Timber

```kotlin
// En Application (PartyHubApp.kt): plantar el arbol de logging
class PartyHubApp : Application() {
    override fun onCreate() {
        super.onCreate()
        Timber.plant(Timber.DebugTree())
    }
}

// En cualquier parte del codigo:
Timber.d("The Mind: jugador $playerId juega carta")
Timber.d("El As: resolviendo ronda")
```

**Ventajas sobre Log.d():**
- No necesita TAG (Timber lo detecta automaticamente del nombre de clase)
- Mas conciso: `Timber.d("msg")` vs `Log.d("TAG", "msg")`
- Sistema de Trees extensible (puedes plantar arboles diferentes para release/debug)

---

## 15. Flujo completo de datos: ejemplo paso a paso

### Ejemplo: Un jugador juega una carta en The Mind

```
1. USUARIO toca "Jugar Carta (12)" en MindGameFragment

2. LISTENER se dispara:
   btnPlayCard.setOnClickListener { viewModel.playCard("0") }

3. VIEWMODEL recibe la llamada:
   fun playCard(playerId: String) {
       val current = _gameState.value ?: return    // Estado actual
       _gameState.value = engine.playCard(current, playerId)  // Delega al engine
   }

4. ENGINE calcula el nuevo estado:
   fun playCard(state: MindGameState, playerId: String): MindGameState {
       // Obtiene la carta mas baja del jugador (12)
       // Compara con la pendiente mas baja (12 == 12 → correcto!)
       // Elimina 12 de la mano del jugador y de pendientes
       // Devuelve NUEVO estado con .copy()
       return state.copy(
           playerHands = nuevoMapa,
           playedCards = listaActualizada,
           pendingCards = pendientesActualizados,
           status = PLAYING  // o REVEALING si todas las manos vacias
       )
   }

5. VIEWMODEL actualiza LiveData:
   _gameState.value = nuevoEstado
   // LiveData notifica a TODOS los observers

6. FRAGMENT observer se ejecuta:
   viewModel.gameState.observe(viewLifecycleOwner) { state ->
       // Actualiza visibilidad de botones
       // Regenera botones de jugador con manos actualizadas
       // Si GAME_OVER o VICTORY, navega a resultados
   }

7. DATABINDING en XML se actualiza automaticamente:
   android:text="@{String.format(@string/mind_label_lives, viewModel.gameState.lives)}"
   // El TextView muestra el nuevo numero de vidas

8. USUARIO ve la UI actualizada
```

---

## 16. Posibles preguntas de entrevista

### Entorno de desarrollo (0,70 pt)
- Que diferencia hay entre compileSdk, targetSdk y minSdk?
- Como se anaden dependencias al proyecto?
- Para que sirve el archivo build.gradle.kts?

### Recursos y diseno (1,50 pt)
- Por que se externalizan los strings a strings.xml?
- Que diferencia hay entre margin y padding?
- Para que sirve dimens.xml? Y colors.xml?
- Que tipos de layout habeis usado y por que?
- Por que usais LinearLayout para el hub en vez de RecyclerView?

### Escuchadores (0,80 pt)
- Como se establece un click listener en un boton?
- Que ventaja tienen las lambdas sobre las clases anonimas?
- Como escalariais si tuvierais 20 botones en vez de 2?

### DataBinding (1,25 pt)
- Que diferencia hay entre ViewBinding y DataBinding?
- Por que usar binding en vez de findViewById()?
- Como funciona DataBinding con LiveData?
- Que hay que configurar para que LiveData funcione con DataBinding? (lifecycleOwner)
- Mostra un ejemplo de expresion @{} en XML
- Cuando elegiriais NO usar DataBinding?

### Intenciones y Navegacion (1,50 pt)
- Que diferencia hay entre Intent explicito e implicito?
- Como se pasan datos entre Activities con Intent?
- Que es el Navigation Component y que ventajas tiene?
- Que es Safe Args y por que se usa?
- Como se navega entre Fragments con Navigation Component?
- Para que sirve android:exported?
- Por que usais Intent para Activities y Navigation Component para Fragments?

### Ciclo de vida (1,00 pt)
- Explica el ciclo de vida de una Activity
- Que diferencia hay entre onPause y onStop?
- Como se guarda estado cuando rota la pantalla?
- Para que sirve savedInstanceState?
- Que diferencia hay entre savedInstanceState y ViewModel para persistir estado?
- Que pasa con el ViewModel cuando rota la pantalla? Y cuando el usuario pulsa atras?

### ViewModel (1,25 pt)
- Para que sirve ViewModel?
- Por que sobrevive a configuration changes pero la Activity no?
- Que es el patron backing property?
- Como se crea y accede a un ViewModel?
- Que NO deberia almacenarse en un ViewModel?
- Cual es la diferencia entre MutableLiveData y LiveData?
- Como funciona ViewModel con LiveData y DataBinding?
- Mostra como se observa un LiveData en un Fragment
- Por que es importante la separacion entre ViewModel y GameEngine?

### Preguntas globales / arquitectura
- Explica como fluyen los datos desde que el usuario toca un boton hasta que la UI se actualiza
- Por que el GameEngine no puede importar android.*?
- Que pasaria si el ViewModel tuviera una referencia a la Activity?
- Explica onSaveInstanceState vs persistencia del ViewModel. Cuando usariais cada uno?
- Habeis usado MVVM. Que responsabilidad tiene cada capa?
