# Referencias e Inspiración

---

## 🔍 Aplicaciones Similares

### 🎮 Jackbox Party Packs
- **Plataforma**: PC, Consolas (móviles como controladores)
- **Descripción**: Colección de minijuegos para fiestas donde cada jugador usa su móvil
- **Qué nos gusta**: 
  - Hub de juegos variados
  - Modo multijugador local perfecto
  - UI divertida y colorida
  - Instrucciones claras antes de cada juego
- **Qué mejoraríamos**: 
  - Requiere dispositivo central (TV/PC) - nuestra app es standalone
  - Requiere internet - nosotros funcionamos offline
- **Inspiración**: Concepto de hub, UI festiva

### 🃏 The Mind (App oficial)
- **Plataforma**: iOS, Android
- **Descripción**: Versión digital del juego de mesa "The Mind"
- **Qué nos gusta**: 
  - Mecánica de sincronización sin comunicación
  - Tensión que genera
  - Simplicidad de reglas
- **Qué mejoraríamos**: 
  - Solo tiene un juego
  - Algunos modos requieren suscripción
- **Inspiración**: Mecánica base del juego The Mind
- **Enlace**: [Play Store](https://play.google.com/store/apps/details?id=com.nswstudios.themind)

### 🎲 Bunch (Group Video Games)
- **Plataforma**: iOS, Android
- **Descripción**: Juegos casuales para jugar con amigos por videollamada
- **Qué nos gusta**: 
  - Colección de juegos variada
  - Interfaz moderna y limpia
  - Fácil de empezar a jugar
- **Qué mejoraríamos**: 
  - Requiere internet y videollamada
  - Enfocada a remoto, no presencial
- **Inspiración**: UI limpia, hub de juegos

### 📱 Plato (Games & Group Chats)
- **Plataforma**: iOS, Android
- **Descripción**: Juegos multijugador con chat integrado
- **Qué nos gusta**: 
  - Gran variedad de juegos clásicos
  - Sistema de amigos y matchmaking
  - Diseño moderno
- **Qué mejoraríamos**: 
  - Enfocada a juego online, no presencial
  - Contiene anuncios y compras in-app
- **Inspiración**: Variedad de juegos, simplicidad

### 🃏 Mus / Guiñote (Apps de cartas españolas)
- **Plataforma**: Android
- **Descripción**: Juegos tradicionales de baraja española
- **Qué nos gusta**: 
  - Mecánicas de baraja española
  - Modo offline disponible
- **Qué mejoraríamos**: 
  - UI anticuada en la mayoría
  - Solo un juego por app
  - Modo multijugador local limitado
- **Inspiración**: Mecánicas de baraja española para "El As"

---

## 🎨 Inspiración de Diseño

### Patrones de UI/UX
| Patrón | Uso en PartyHub |
|--------|-----------------|
| **Cards/Tarjetas** | Mostrar juegos en el hub como tarjetas grandes |
| **Bottom Navigation** | Navegación principal (Hub, Configuración, Info) |
| **Onboarding Carousel** | Tutorial de reglas antes de cada juego |
| **Full-screen dialogs** | Pantallas de "pasa el teléfono", resultados |
| **Floating Action Button** | Iniciar partida rápida |

### Paleta de Colores Propuesta
| Color | Hex | Uso |
|-------|-----|-----|
| Principal | `#6200EE` | Botones principales, acentos |
| Secundario | `#03DAC6` | Elementos interactivos |
| Fondo | `#121212` | Theme oscuro |
| Superficie | `#1E1E1E` | Tarjetas, diálogos |
| Error | `#CF6679` | Feedback negativo, vidas perdidas |
| Éxito | `#4CAF50` | Feedback positivo |

### Tipografía
- **Fuente principal**: Roboto (default Android)
- **Títulos**: Bold, grande
- **Cuerpo**: Regular, legible
- **Números (juegos)**: Monospace o extra bold para destacar

---

## 📚 Recursos Técnicos

### Comunicación LAN
- **NSD (Network Service Discovery)**: Para descubrimiento de dispositivos
- **Sockets TCP**: Para comunicación fiable
- **UDP Broadcast**: Para anunciar salas

### Librerías Potenciales
| Librería | Propósito |
|----------|-----------|
| **Kotlin Coroutines** | Asincronía y concurrencia |
| **ViewModel + LiveData** | Arquitectura MVVM |
| **Navigation Component** | Navegación entre pantallas |
| **Material Design 3** | Componentes de UI modernos |
| **Lottie** | Animaciones fluidas |

### Documentación Clave
- [Material Design 3 Guidelines](https://m3.material.io/)
- [Android Network Service Discovery](https://developer.android.com/training/connect-devices-wirelessly/nsd)
- [Kotlin Coroutines](https://kotlinlang.org/docs/coroutines-overview.html)

---

## 💡 Ideas de Juegos Adicionales

### Para implementar si hay tiempo
| Juego | Descripción | Complejidad |
|-------|-------------|-------------|
| **Ruleta de retos** | Gira y cumple el reto | Baja |
| **Piedra, papel, tijera** | Torneo eliminatorio | Baja |
| **Dados mentiroso** | Farolear con dados | Media |
| **Quién es más probable** | Votar quién haría X | Baja |
| **Tabú express** | Adivinar palabras | Media |

### Criterios de selección
- ✅ Funciona bien presencialmente
- ✅ Reglas simples
- ✅ Partidas cortas (< 10 min)
- ✅ Adaptable a local y LAN

---

## 📊 Análisis Competitivo

| App | Offline | Local | LAN | Hub | Gratis |
|-----|---------|-------|-----|-----|--------|
| Jackbox | ❌ | ✅ | ❌ | ✅ | ❌ |
| The Mind | ✅ | ❌ | ✅ | ❌ | ⚠️ |
| Bunch | ❌ | ❌ | ❌ | ✅ | ✅ |
| Plato | ❌ | ❌ | ❌ | ✅ | ⚠️ |
| **PartyHub** | ✅ | ✅ | ✅ | ✅ | ✅ |

**Diferenciador clave**: Única app que combina modo local + LAN + offline + hub de juegos.

---

**Última actualización**: 6 de Febrero de 2026
