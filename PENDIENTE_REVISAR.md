# Cosas a revisar antes de la siguiente entrega

Estado del repo `app/` en `main` tras el merge de `feature/lan` (`8da611c`).

---

## 1. `SettingsActivity` no declarada en el Manifest

**Archivo:** `app/src/main/java/com/partyhub/SettingsActivity.kt`

Existe la clase pero **no está en `AndroidManifest.xml`**. Nadie la lanza actualmente, así que no provoca crash, pero si en algún momento se intenta abrir con un `Intent` explícito, la app peta.

**Opciones:**
- Borrarla si es dead code (parece que sí — la configuración real va por `SettingsFragment` dentro del NavGraph).
- O añadirla al Manifest si se piensa usar.

---

## 2. `isDarkModeEnabled` y `getPlayerAlias` duplicados

**Archivos afectados:**
- `com.partyhub.feature.settings.SettingsFragment` (companion object)
- `com.partyhub.SettingsActivity` (companion object)

Ambas clases definen los mismos métodos estáticos (`isDarkModeEnabled`, `getPlayerAlias`). `PartyHubApp` ya llama al de `SettingsFragment`, que es el correcto. La versión en `SettingsActivity` es redundante y puede llevar a confusión si alguien la importa por error.

**Solución:** Si se borra `SettingsActivity` (ver punto 1), este problema desaparece solo.

---

## 3. Navegación LAN → juego con Safe Args (probar en dispositivo)

**Flujo:** `LanLobbyFragment` → `mindGameFragment` / `asGameFragment` directamente, saltándose los fragments de configuración.

Los IDs `mindGameFragment` y `asGameFragment` están en los nested graphs (`nav_mind`, `nav_as`), incluidos en `nav_main`. Debería funcionar, pero hay que verificar en ejecución que:

- Safe Args genera bien los argumentos al navegar desde el lobby LAN.
- No se produce un `IllegalArgumentException` por destino no encontrado.

**Cómo probar:** Crear una sala LAN como host, unirse desde otro dispositivo y pulsar "Iniciar partida". Si llega a la pantalla de juego sin crash, está bien.
