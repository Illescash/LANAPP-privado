# PartyHub - Seguimiento de Aprendizaje (Entrega 2)

**Fecha de inicio:** 8 de abril de 2026  
**Objetivo:** Registrar qué conceptos domino y cuáles necesito reforzar

---

## 📚 Temas del Documento de Estudio

| # | Tema | ✓ | ✗ | Notas |
|----|------|---|---|-------|
| 1 | Visión general de la app | ✅ | | The Mind (cooperativo) y El As (competitivo), flujo Hub → Juego |
| 2 | Configuración del proyecto (Gradle, SDK) | ⚠️ | | Menos importante para DADM (referencia) |
| 3 | Arquitectura MVVM | ✅ | | **Model (GameEngine) → ViewModel (LiveData) → View (Fragment)**. Separación clara. |
| 4 | Activities: contenedores de navegación | ✅ | | Contenedores mínimos, solo `setContentView()`. NavHostFragment para navegar. |
| 5 | Fragments: la capa de vista | ✅ | | **Pattern `_binding` nullable / `binding` non-null**. Cleanup en `onDestroyView()` (memory leak). |
| 6 | ViewBinding y DataBinding | ✅ | | **DataBinding**: `@{}` en XML observa ViewModel automáticamente. Simple vs lógica compleja. |
| 7 | ViewModel y LiveData | ✅ | | **MutableLiveData privada** → **LiveData pública**. Backing property pattern. observe() reacciona automáticamente. |
| 8 | GameEngines: lógica pura Kotlin | ✅ | | Sin imports `android.*`. Recibe estado + acción → devuelve nuevo estado (inmutable). |
| 9 | Navegación: Intents, NavGraph, Safe Args | ⬜ | ⬜ | *A completar cuando sea necesario* |
| 10 | Ciclo de vida y estado | ✅ | | **CRÍTICO**: Fragment muere en rotación, ViewModel sobrevive. ViewModelProvider guarda el estado. |
| 11 | Escuchadores (Listeners) | ✅ | | `setOnClickListener {}` en Fragment delega a ViewModel. Patrón estándar. |
| 12 | Recursos XML (layouts, colors, strings) | ⚠️ | | Menos crítico (referencia) |
| 13 | AndroidManifest | ⚠️ | | Menos crítico (referencia) |
| 14 | Logging con Timber | ✅ | | `Timber.d()` en ViewModel + Engine para debugging. Simple. |
| 15 | Flujo completo de datos (ejemplo paso a paso) | ✅ | | **Usuario → Fragment.onClick() → ViewModel.accion() → Engine.calcular() → LiveData.value = nuevo → observe() → UI actualiza** |
| 16 | Preguntas de entrevista | ✅ | | ¿Por qué ViewModel sobrevive rotaciones? ¿Qué es LiveData? ¿DataBinding vs observe()? |

**Leyenda:**
- ✓ = Lo entiendo
- ✗ = No lo entiendo
- ⬜ = Sin revisar aún

---

## 🎮 Conceptos Clave del Proyecto

### Los Juegos
- [x] The Mind (mecánica cooperativa, 2-4 jugadores, ordenar cartas 1-100)
- [x] El As (mecánica competitiva, 3-6 jugadores, baraja española)
- [x] Diferencias: The Mind es colaborativo, El As es contra otros

### Arquitectura
- [x] MVVM: Model (Engine) + View (Fragment) + ViewModel (intermediario)
- [x] Separación: GameEngine (Kotlin puro, sin Android) vs ViewModel (reactivo) vs Fragment (UI)
- [x] Flujo: Usuario → Fragment.onClick → ViewModel.accion() → Engine.calcular() → LiveData.value → observe() → UI actualiza

### Componentes Android
- [x] **Activity**: contenedor mínimo, solo setContentView()
- [x] **Fragment**: UI real, usa ViewBinding, observa ViewModel
- [x] **ViewModel**: guarda estado, no se destruye en rotación, delega al Engine
- [x] **LiveData**: datos que notifican cambios automáticamente, observe() reacciona
- [x] **ViewBinding**: generado automáticamente, acceso tipado a vistas
- [x] **DataBinding**: XML observa ViewModel directamente con `@{}`

### Ciclo de Vida (CRÍTICO)
- [x] Fragment.onCreateView → ViewModel se crea (primera vez) o reutiliza
- [x] Fragment.onViewCreated → se registran observe() y listeners
- [x] **ROTACIÓN**: Fragment.onDestroyView ejecuta, ViewModel sigue vivo
- [x] Fragment.onCreateView (nuevo) → reutiliza ViewModel anterior
- [x] `viewLifecycleOwner` es crucial para observe() (no usar `this`)

---

## 💡 Dudas Actuales

✅ **Resueltas:**
- ✅ Diferencia entre DataBinding y observe()
- ✅ Cómo observe() y DataBinding se disparan al inicio
- ✅ Por qué ViewModel sobrevive a rotaciones

❓ **Pendientes:**
- [ ] Navegación (Intents, NavGraph, Safe Args) — si es necesario
- [ ] Detalles de Recursos XML — si lo necesitas

---

## 📝 Notas Personales

**Lo más importante que he aprendido:**

1. **MVVM es separación clara**: Engine (lógica) ≠ ViewModel (estado) ≠ Fragment (UI)
2. **LiveData = "aviso automático"**: cuando cambia, los observers reaccionan sin refresh manual
3. **Ciclo de vida es el truco**: Fragment se destruye en rotación, ViewModel sigue vivo
4. **DataBinding vs observe()**: 
   - XML simple → DataBinding automático
   - Lógica compleja → observe() en Kotlin
5. **observe() y DataBinding escuchan igual**: ambos reaccionan a cada cambio de LiveData

---

## 🔧 Tareas de Aprendizaje

- [x] Leer ESTUDIO_ENTREGA2.md (parcialmente, lo crítico)
- [x] Entender la estructura de carpetas del proyecto
- [x] Ver el código de un juego (MindGameFragment + MindViewModel + MindGameEngine)
- [x] Entender cómo el ViewModel habla con el GameEngine
- [x] Ver un ejemplo completo: Fragment → ViewModel → GameEngine → Resultado
- [x] Entender ciclo de vida (rotación, ViewModelProvider)
- [x] Entender LiveData (observe(), backing property)
- [x] Entender DataBinding (`@{}` en XML)
- [ ] **Siguiente (si necesario)**: Navegación (Intents, NavGraph, Safe Args)

---

**Última actualización:** 8 de abril de 2026
