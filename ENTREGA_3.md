# Entrega 3 - PartyHub 🎮

**Asignatura:** Desarrollo de Aplicaciones para Dispositivos Móviles (DADM)
**Curso:** 2025-2026 · **Docente:** Alicia Garrido Peña
**Equipo:** Rafael Romero Monzón · Diego Illescas Lasa
**Presentación en clase:** 8 y 13 de mayo de 2026 · **Peso:** 70% de la nota final

> 📋 Rúbrica oficial: [`ENTREGA3/Rubrica_2025-2026_E3.pdf`](./ENTREGA3/Rubrica_2025-2026_E3.pdf)
> 📚 Contenido nuevo del curso (RecyclerView, Material, menús, preferencias, Room, Firebase, Retrofit, periféricos): [`ENTREGA3/UAMX2.txt`](./ENTREGA3/UAMX2.txt)

---

## Alcance de la Entrega

Versión final de PartyHub. Sobre la base de E2 (hub + The Mind + El As en local con MVVM, ViewBinding/DataBinding, Navigation, intents y LiveData) hay que añadir todo el material visto en UAMx 2:

1. **RecyclerView + Material Design** en las pantallas que listan elementos (juegos del hub, historial, lista de jugadores LAN…).
2. **Menús y preferencias** (drawer o bottom nav, menú de toolbar, pantalla de settings).
3. **Base de datos Room** con LiveData para persistir el historial de partidas.
4. **Firebase** (Auth + Firestore como backup en la nube del historial).
5. **Modo LAN** (extra principal): jugar en la misma red Wi-Fi, varios dispositivos, sin internet.
6. **Extras secundarios**: dark mode, layouts alternativos (land/sw600dp), permisos en runtime…

La app debe quedar **completa, estable y presentable** los días 8 y 13 de mayo.

---

## Criterios de Evaluación (Rúbrica E3)

| # | Criterio | Peso | Cómo lo cubrimos en PartyHub |
|---|----------|------|------------------------------|
| 1 | **Diseño de la app** | **2,20** | RecyclerView en hub e historial · Material (Cards, FAB, TextInputLayout, Toolbar, ToggleGroup) · justificación de cada vista en el README |
| 2 | Intenciones y Navegación | 1,00 | Mantener intents E2 + reorganizar navegación con NavGraph único + drawer/bottom nav |
| 3 | Ciclo de vida | 0,50 | Documentar y probar `onSaveInstanceState` en todas las activities/fragments con partida en curso |
| 4 | ViewModel y LiveData | 1,00 | Mantener VM existentes + nuevos VM para auth, historial y LAN, todos con backing property |
| 5 | Menús y preferencias | 0,80 | Toolbar con menú (settings, logout, share) · `SettingsActivity` con `SwitchPreferenceCompat` y `ListPreference` · justificación SharedPreferences vs Room |
| 6 | Base de datos Room | 1,50 | `MatchHistory` (entidad) + `MatchDao` con `LiveData` + `PartyHubDatabase` + factory de VM |
| 7 | **Firebase y extras** | **2,50** | Firebase Auth (email/password) + Firestore como backup del historial · **Modo LAN** (extra mayor) · permisos runtime |
| 8 | Control de versiones | 0,50 | Commits pequeños y frecuentes · Conventional Commits · ramas `feature/*` |
| | **TOTAL** | **10,00** | |

---

## Arquitectura objetivo (E3)

```
PartyHubApplication                     ← inicializa Room, Firebase, Timber, prefs
├── database (Room)
│   ├── PartyHubDatabase
│   ├── MatchHistory @Entity
│   └── MatchDao (LiveData<List<MatchHistory>>)
├── auth
│   ├── AuthRepository (FirebaseAuth)
│   └── AuthViewModel
├── network (LAN)
│   ├── DiscoveryService (UDP broadcast)
│   ├── LanServer / LanClient (TCP + JSON)
│   └── LobbyViewModel
└── repository
    └── HistoryRepository (Room ⇄ Firestore)

MainActivity (host único)
├── DrawerLayout / BottomNavigationView con NavController
├── AuthFragment (login/registro Firebase)
├── HubFragment (RecyclerView de juegos + Material Cards)
├── HistoryFragment (RecyclerView de partidas, LiveData de Room)
├── LanLobbyFragment (descubrir host, unirse, lista de jugadores)
├── SettingsActivity (preferences XML)
└── feature/{themind,elas}/  ← se mantienen, adaptados a Navigation y modo LAN
```

### Reglas técnicas (alineadas con UAMx 2)

- **RecyclerView**: con DataBinding en `list_item_*.xml`, `ViewHolder` con `inner class` y constructor recibiendo el binding (no la view).
- **Material Design**: `Theme.MaterialComponents.DayNight.DarkActionBar`, `MaterialCardView`, `FloatingActionButton`, `TextInputLayout`, `MaterialButtonToggleGroup`.
- **Menús**: `MenuProvider` en fragments (no `setHasOptionsMenu`), `setupWithNavController` en bottom/drawer.
- **Preferencias**: `SettingsActivity` + `root_preferences.xml`, accedidas con `PreferenceManager.getDefaultSharedPreferences`. Companion object con constantes y getters.
- **Room**: entidad con `@PrimaryKey(autoGenerate = true)`, DAO con `suspend` para escrituras y `LiveData` para lecturas, `fallbackToDestructiveMigration()`, singleton en companion object.
- **Coroutines**: solo donde Room/Firebase lo exijan (`viewModelScope.launch`, `applicationScope` en Application). El resto del código sigue con LiveData como en E2.
- **Firebase**: `google-services.json` en `app/`, `FirebaseApp.initializeApp` en `PartyHubApplication`. Auth con `email/password`, Firestore para `match_history`.
- **LAN**: TCP para juego, UDP broadcast para descubrimiento, JSON manual con `org.json` (evitamos kotlinx.serialization para no salir de lo visto). Permiso `INTERNET` y `ACCESS_NETWORK_STATE`.
- **Prohibido en código**: cualquier referencia a IA/Claude/Copilot (ver CLAUDE.md).

---

## Plan de Commits

**Convención** (Conventional Commits, igual que E2):
```
feat(scope): …
fix(scope): …
refactor(scope): …
style(scope): …
docs(scope): …
chore(scope): …
```

> 🔁 **Importante**: tras cada commit real, actualizar la columna **Estado** (`⬜` → `✅ <hash>`). Si lo hace alguien distinto al responsable planificado, anotarlo: `✅ abc1234 (Rafael)`. Las fechas son orientativas.

---

### Fase 0 — Preparación (29-30 abr)

| # | Commit | Responsable | Notas | Estado |
|---|--------|-------------|-------|--------|
| 0.1 | `chore(gradle): actualizar libs.versions.toml para E3 (room, firebase BOM, material, lifecycle)` | Diego | Añadir `room`, `firebase-bom`, `kotlinx-coroutines-play-services`, `kotlin-kapt` plugin | ✅ |
| 0.2 | `chore(git): crear rama develop y feature/* para E3` | Rafael | `develop` ← `main`; ramas `feature/recycler`, `feature/material`, `feature/menu-prefs`, `feature/room`, `feature/firebase`, `feature/lan` | ✅ |
| 0.3 | `docs(entrega3): añadir ENTREGA_3.md con plan de commits` | Diego | Este documento | ✅ |

---

### Fase 1 — RecyclerView + Material Design (rúbrica §1, 2,20 pts) — 30 abr → 3 may

Rama: `feature/recycler` y `feature/material`.

| # | Commit | Responsable | Notas | Estado |
|---|--------|-------------|-------|--------|
| 1.1 | `refactor(hub): convertir HubFragment a RecyclerView con MaterialCardView` | Rafael | Crear `GameAdapter`, `list_item_game.xml` con DataBinding (`<variable name="game">`), `ViewHolder(binding)`. Modelo `GameInfo(id, name, iconRes, minPlayers, maxPlayers)` | ⬜ |
| 1.2 | `feat(hub): añadir FloatingActionButton para modo LAN dentro de CoordinatorLayout` | Rafael | Snackbar de feedback al pulsar | ⬜ |
| 1.3 | `style(theme): migrar a Theme.MaterialComponents.DayNight.DarkActionBar` | Diego | Definir `colorPrimary`, `colorSecondary`, `colorSurface` en `values/colors.xml` y `values-night/colors.xml` | ⬜ |
| 1.4 | `style(ui): sustituir EditText por TextInputLayout + TextInputEditText en config de juegos` | Diego | `MindConfigFragment` y `AsConfigFragment` | ⬜ |
| 1.5 | `style(ui): usar MaterialButtonToggleGroup para selección de dificultad en The Mind` | Rafael | `singleSelection=true`, listener `addOnButtonCheckedListener` | ⬜ |
| 1.6 | `feat(themind): mostrar mano del jugador con RecyclerView` | Rafael | Ya hay lista de cartas; pasarla a RV con `list_item_card.xml` + DataBinding | ⬜ |
| 1.7 | `feat(elas): mostrar jugadores activos con RecyclerView` | Diego | `list_item_player.xml` con vidas e indicador de turno | ⬜ |
| 1.8 | `style(ui): aplicar textAppearance Material y elevation a tarjetas` | Diego | `?attr/textAppearanceTitleLarge`, `cardElevation`, `cardCornerRadius` | ⬜ |

---

### Fase 2 — Navegación unificada, menús y preferencias (rúbrica §2, §5) — 3-5 may

Rama: `feature/menu-prefs`.

| # | Commit | Responsable | Notas | Estado |
|---|--------|-------------|-------|--------|
| 2.1 | `refactor(nav): unificar navegación en MainActivity con NavGraph único` | Diego | Eliminar `TheMindActivity` y `ElAsActivity`; un solo `MainActivity` con `NavHostFragment`. Las activities pasan a ser fragments dentro del mismo grafo | ⬜ |
| 2.2 | `feat(nav): añadir BottomNavigationView con setupWithNavController` | Diego | Items: Hub, Historial, Ajustes. Menú `bottom_nav_menu.xml` | ⬜ |
| 2.3 | `feat(nav): añadir DrawerLayout con NavigationView y cabecera personalizada` | Rafael | Alternativa al bottom nav para tablets; `nav_header.xml` con logo. Activable según `sw600dp` | ⬜ |
| 2.4 | `feat(menu): toolbar con MenuProvider (compartir, settings, logout)` | Rafael | `MenuProvider` registrado en `onCreateView` de `HubFragment` y `HistoryFragment` | ⬜ |
| 2.5 | `feat(settings): crear SettingsActivity con root_preferences.xml` | Diego | `SwitchPreferenceCompat` (modo oscuro, sonido), `ListPreference` (tema/idioma), `EditTextPreference` (alias del jugador) | ⬜ |
| 2.6 | `feat(settings): persistir alias de jugador y tema con DefaultSharedPreferences` | Diego | Companion object con `getPlayerAlias`, `setPlayerAlias`, `getNightMode`. Aplicar `AppCompatDelegate.setDefaultNightMode` al cambiar | ⬜ |
| 2.7 | `feat(settings): aplicar setDefaultValues en Application.onCreate` | Diego | `PreferenceManager.setDefaultValues(this, R.xml.root_preferences, false)` | ⬜ |
| 2.8 | `docs(readme): justificar uso de SharedPreferences para alias/tema vs Room para historial` | Rafael | En el README del repo `app/`: SP para datos pequeños clave-valor; Room para datos estructurados consultables | ⬜ |

---

### Fase 3 — Base de datos Room (rúbrica §6, 1,50 pts) — 5-7 may

Rama: `feature/room`.

| # | Commit | Responsable | Notas | Estado |
|---|--------|-------------|-------|--------|
| 3.1 | `chore(gradle): añadir plugin kotlin-kapt y dependencias de Room` | Rafael | `room-runtime`, `room-ktx`, `room-compiler` (kapt) en versiones de `libs.versions.toml` | ⬜ |
| 3.2 | `feat(db): crear entidad MatchHistory (@Entity) con id, game, players, winner, durationMs, finishedAt` | Rafael | `@PrimaryKey(autoGenerate = true) val id: Long = 0`. `players` se guarda como String separado por comas (no relaciones, no visto en clase) | ⬜ |
| 3.3 | `feat(db): crear MatchDao con suspend insert/delete y LiveData<List<MatchHistory>>` | Diego | `@Query("SELECT * FROM match_history ORDER BY finishedAt DESC")`, `@Query("DELETE FROM match_history")` | ⬜ |
| 3.4 | `feat(db): crear PartyHubDatabase con companion getInstance singleton y fallbackToDestructiveMigration` | Diego | Versión 1, `exportSchema = false` | ⬜ |
| 3.5 | `feat(app): exponer database como propiedad lazy en PartyHubApplication` | Rafael | Registrar la `Application` en `AndroidManifest.xml` con `android:name=".PartyHubApplication"` | ⬜ |
| 3.6 | `feat(history): crear HistoryViewModel y HistoryViewModelFactory` | Rafael | VM recibe `MatchDao`, expone `val matches = dao.getAll()`, escrituras con `viewModelScope.launch` | ⬜ |
| 3.7 | `feat(history): crear HistoryFragment con RecyclerView observando LiveData del VM` | Diego | `adapter.data = emptyList()` antes del observe; al recibir, `notifyDataSetChanged()` | ⬜ |
| 3.8 | `feat(history): registrar partida al terminar The Mind y El As` | Diego | En `MindResultFragment` y `AsResultFragment`, llamar a `historyViewModel.addMatch(...)` desde el botón "Guardar" | ⬜ |
| 3.9 | `feat(history): añadir menú con "Borrar historial" usando MenuProvider` | Rafael | Confirmación con `MaterialAlertDialog` antes de `dao.removeAll()` | ⬜ |

---

### Fase 4 — Firebase Auth + Firestore (rúbrica §7, parte) — 7-9 may

Rama: `feature/firebase`. **Requiere cuenta Google del equipo y `google-services.json` (no commitear secretos personales — añadir al `.gitignore` del repo `app/` si contiene claves sensibles, o documentar cómo regenerarlo).**

| # | Commit | Responsable | Notas | Estado |
|---|--------|-------------|-------|--------|
| 4.1 | `chore(firebase): registrar app en Firebase Console y añadir google-services.json` | Diego | Crear proyecto `partyhub-dadm`, registrar paquete `com.partyhub`, descargar JSON | ⬜ |
| 4.2 | `chore(gradle): añadir plugin google-services y BOM de Firebase` | Diego | Project: `id("com.google.gms.google-services") apply false`. Module: `firebase-bom`, `firebase-auth-ktx`, `firebase-firestore-ktx`, `kotlinx-coroutines-play-services` | ⬜ |
| 4.3 | `feat(auth): crear AuthRepository con register/login/logout devolviendo Result<FirebaseUser>` | Rafael | Usar `.await()` de `kotlinx-coroutines-play-services` | ⬜ |
| 4.4 | `feat(auth): crear AuthViewModel con LiveData<FirebaseUser?> y LiveData<String> error` | Rafael | Backing property pattern; `init { _user.value = repository.currentUser() }` | ⬜ |
| 4.5 | `feat(auth): crear AuthFragment como destino inicial del NavGraph` | Diego | Dos `TextInputLayout` (email, password) + botones registro/login. Si `user != null`, navigate al hub y `popBackStack` | ⬜ |
| 4.6 | `feat(auth): añadir logout en menú principal que vuelve a AuthFragment` | Diego | Limpiar pila de navegación con `popBackStack(R.id.authFragment, false)` | ⬜ |
| 4.7 | `feat(history): crear HistoryRepository sincronizando Room con Firestore` | Rafael | `addMatch`: insert en DAO + `collection.document(id).set(toMap())`. `syncFromFirestore(scope)` con `addSnapshotListener` | ⬜ |
| 4.8 | `refactor(history): HistoryViewModel usa HistoryRepository en vez de DAO directo` | Rafael | Factory recibe el repository en lugar del DAO. Init dispara la sincronización | ⬜ |
| 4.9 | `feat(firestore): segmentar match_history por uid de usuario` | Diego | Subcolección `users/{uid}/matches`. Reglas: `allow read, write: if request.auth != null && request.auth.uid == userId` | ⬜ |

---

### Fase 5 — Modo LAN (extra principal de rúbrica §7) — 9-12 may

Rama: `feature/lan`. Es la funcionalidad estrella del proyecto y vale como "funcionalidad extra" (1,00 pt) + "desarrollos específicos" (1,00 pt).

| # | Commit | Responsable | Notas | Estado |
|---|--------|-------------|-------|--------|
| 5.1 | `chore(manifest): añadir permisos INTERNET y ACCESS_NETWORK_STATE` | Diego | También `ACCESS_WIFI_STATE` para mostrar SSID en la UI | ⬜ |
| 5.2 | `feat(lan): crear DiscoveryService con UDP broadcast para anunciar/detectar host` | Rafael | Puerto fijo (p.ej. 8888). Mensaje JSON `{"app":"partyhub","host":"Rafa","ip":"…","port":9999}` | ⬜ |
| 5.3 | `feat(lan): crear LanServer (Host) con ServerSocket y un hilo por cliente` | Rafael | Acepta conexiones, mantiene `List<ClientConnection>`, broadcast de mensajes a todos | ⬜ |
| 5.4 | `feat(lan): crear LanClient con Socket y lectura en hilo separado` | Diego | Envía/recibe líneas JSON por `BufferedReader/Writer` | ⬜ |
| 5.5 | `feat(lan): definir protocolo de mensajes (JOIN, STATE, ACTION, LEAVE) con org.json` | Diego | Documentar en `app/README.md` | ⬜ |
| 5.6 | `feat(lan): crear LobbyViewModel con LiveData de jugadores conectados y rol (HOST/CLIENT)` | Rafael | Backing property con `_players: MutableLiveData<List<PlayerInfo>>` | ⬜ |
| 5.7 | `feat(lan): crear LanLobbyFragment con RecyclerView de jugadores + botones "Crear sala" / "Unirse"` | Diego | Bottom sheet con SSID actual y lista de hosts descubiertos | ⬜ |
| 5.8 | `refactor(themind): adaptar MindGameEngine para recibir acciones desde LAN o desde UI local` | Rafael | El `GameEngine` sigue siendo lógica pura; el VM decide si la acción la origina la UI o llega por `LanClient` | ⬜ |
| 5.9 | `refactor(elas): adaptar AsGameEngine para sincronización LAN (host es fuente de verdad)` | Diego | Solo el host ejecuta la engine; los clientes envían `ACTION` y reciben `STATE` | ⬜ |
| 5.10 | `feat(lan): cerrar sockets en onDestroy del VM (onCleared) y al pulsar atrás` | Rafael | Importante para no dejar puertos colgados al rotar pantalla | ⬜ |
| 5.11 | `fix(lan): manejar desconexión de cliente y caída del host con Snackbar de aviso` | Diego | Volver al lobby si cae el host | ⬜ |

---

### Fase 6 — Ciclo de vida + extras secundarios (rúbrica §3 + §7 secundarios) — 12 may

Rama: `feature/polish`.

| # | Commit | Responsable | Notas | Estado |
|---|--------|-------------|-------|--------|
| 6.1 | `fix(lifecycle): persistir estado de juego en onSaveInstanceState para todas las pantallas` | Diego | The Mind (cartas jugadas, nivel), El As (mano, vidas, turno) | ⬜ |
| 6.2 | `feat(theme): añadir values-night/themes.xml y values-night/colors.xml para dark mode` | Rafael | Forzable desde Settings con `AppCompatDelegate.setDefaultNightMode` | ⬜ |
| 6.3 | `feat(layout): añadir layout-land y layout-sw600dp para HubFragment e HistoryFragment` | Rafael | Tablets: dos paneles (lista + detalle). Landscape: grid en lugar de lista | ⬜ |
| 6.4 | `feat(i18n): añadir traducciones values-en/strings.xml usando Translations Editor` | Diego | Mínimo: hub, settings, errores de auth, mensajes LAN | ⬜ |
| 6.5 | `style(launcher): personalizar ic_launcher con vector asset propio` | Cualquiera | Ya pendiente de E2 (C4) | ⬜ |
| 6.6 | `feat(share): mantener intent implícito ACTION_SEND en pantallas de resultado` | Diego | Verificar que sigue funcionando tras unificar navegación | ⬜ |

---

### Fase 7 — Documentación y entrega (rúbrica §8) — 13 may (mañana)

| # | Commit | Responsable | Notas | Estado |
|---|--------|-------------|-------|--------|
| 7.1 | `docs(readme): documentar arquitectura E3, Room, Firebase y modo LAN en app/README.md` | Rafael | Diagramas, justificación de cada vista (rúbrica §1.1), ciclo de vida (§3) | ⬜ |
| 7.2 | `docs(entrega3): justificar uso de fragments vs activities y posición de cada intent` | Diego | Sección dedicada en `ENTREGA_3.md` o en `app/README.md` (rúbrica §2) | ⬜ |
| 7.3 | `docs(entrega3): describir ciclo de vida de cada fragment principal` | Rafael | onCreate / onCreateView / onViewCreated / onSaveInstanceState / onDestroyView (rúbrica §3) | ⬜ |
| 7.4 | `chore(release): bump versionName a 3.0 y crear tag v3.0` | Diego | `git tag -a v3.0 -m "Entrega 3"` en el repo `app/` | ⬜ |
| 7.5 | `chore(merge): merge de develop a main tras presentación` | Cualquiera | Tras presentar el día 13 | ⬜ |

---

## Justificaciones que hay que tener listas para la presentación

La rúbrica puntúa varias veces "justificación y comprensión". Toca poder defender oralmente:

1. **Por qué cada vista** (RV vs LinearLayout, Coordinator vs Constraint, Toolbar vs ActionBar nativa, Drawer vs Bottom).
2. **Por qué fragmentos vs activities** (un host = `MainActivity`; el resto fragments para compartir VM y NavGraph).
3. **Dónde y por qué cada intent** (explícito a SettingsActivity por aislamiento; implícito ACTION_SEND para compartir resultado; sin intents para navegación interna porque la cubre Navigation).
4. **Ciclo de vida** de cada activity/fragment, especialmente `onSaveInstanceState` en partidas en curso.
5. **SharedPreferences vs Room vs Firestore**: clave-valor pequeño · estructurado consultable · backup en la nube.
6. **Por qué LAN como extra** (el proyecto se diseñó multijugador presencial desde E1, sin internet → encaja con "desarrollo específico" de la rúbrica).

---

## Ramas Git

```
main
├── develop
│   ├── feature/recycler
│   ├── feature/material
│   ├── feature/menu-prefs
│   ├── feature/room
│   ├── feature/firebase
│   ├── feature/lan
│   └── feature/polish
```

Convención: PRs (o merges directos si trabajamos en local) `feature/* → develop`, y al final `develop → main` con tag `v3.0`.

---

## Cómo continuar el trabajo

Cuando alguien retome el proyecto:

1. `git pull` en ambos repos (`Apps móviles/` y `Apps móviles/app/`).
2. Mirar la primera fila con `⬜` en este documento, en la fase más temprana.
3. Crear/cambiar a la rama correspondiente (`git checkout -b feature/<...>` desde `develop`).
4. Implementar el commit, **probar que compila en Android Studio** antes de commitear.
5. Mostrar al compañero (o al agente) el mensaje de commit y los ficheros antes de ejecutar `git commit`.
6. Tras commitear, actualizar la columna **Estado** de la fila correspondiente con el hash corto.
7. Si se añaden commits no planificados, añadir una fila nueva en la fase correspondiente.

> No hay que respetar fechas exactas: lo que importa es no saltarse fases (no entrar en Firebase sin Room, no entrar en LAN sin que el modo local esté estable).

---

## Diferido a hipotético post-entrega

No entra en E3, pero queda anotado por si se quisiera ampliar:

- Jetpack Compose (la profe lo menciona como futurible).
- Firebase Cloud Messaging para notificar invitaciones a salas.
- Bluetooth como alternativa a LAN.
- Tests instrumentados de Room y unit tests de las engines.
