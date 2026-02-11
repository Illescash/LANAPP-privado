# Requisitos No Funcionales

---

## 📋 Definición

Los requisitos no funcionales describen **cómo debe funcionar** PartyHub. Se refieren a características de calidad, rendimiento, seguridad, usabilidad y compatibilidad.

---

## 🚀 Rendimiento

### RNF-01: Tiempo de Carga Inicial
- **Requisito**: La aplicación debe cargar en menos de 3 segundos
- **Métrica**: Tiempo desde apertura hasta hub principal funcional
- **Prioridad**: Alta

### RNF-02: Fluidez de la Interfaz
- **Requisito**: La interfaz debe mantener al menos 30 FPS durante animaciones
- **Aplicable a**: Transiciones, animaciones de cartas, revelación de números
- **Prioridad**: Alta

### RNF-03: Latencia en Modo LAN
- **Requisito**: Las acciones deben sincronizarse en menos de 500ms
- **Crítico para**: The Mind (timing preciso) y El As (turnos)
- **Prioridad**: Alta

### RNF-04: Consumo de Recursos
- **Requisito**: Uso eficiente de memoria y batería
- **Criterios**:
  - Memoria RAM: < 150 MB en uso normal
  - Batería: Sin drenaje notable durante partidas
- **Prioridad**: Media

---

## 🎨 Usabilidad

### RNF-05: Interfaz Intuitiva
- **Requisito**: Un usuario nuevo debe poder jugar sin tutorial obligatorio
- **Métrica**: Completar primera partida en < 3 minutos sin ayuda
- **Prioridad**: Alta

### RNF-06: Botones Grandes y Claros
- **Requisito**: Los elementos interactivos deben ser fáciles de tocar
- **Criterios**:
  - Tamaño mínimo de botones: 48dp x 48dp
  - Separación entre elementos táctiles: > 8dp
- **Notas**: Especialmente importante en modo local con varios jugadores
- **Prioridad**: Alta

### RNF-07: Feedback Visual y Háptico
- **Requisito**: Toda acción debe tener feedback inmediato
- **Criterios**:
  - Animaciones de confirmación
  - Vibración opcional en eventos importantes
  - Sonidos para acciones clave
- **Prioridad**: Media

### RNF-08: Mensajes Claros
- **Requisito**: Los errores y estados deben comunicarse de forma comprensible
- **Criterios**:
  - Mensajes en español
  - Sin códigos técnicos expuestos al usuario
  - Instrucciones de resolución cuando aplique
- **Prioridad**: Alta

### RNF-09: Accesibilidad Básica
- **Requisito**: Cumplir con estándares básicos de accesibilidad Android
- **Criterios**:
  - Contraste adecuado (WCAG AA)
  - Textos legibles (mínimo 14sp)
  - Soporte para TalkBack (opcional)
- **Prioridad**: Baja

---

## 📱 Compatibilidad

### RNF-10: Versión Mínima de Android
- **Requisito**: Soporte para Android 7.0 (API 24) en adelante
- **Cobertura**: ~85% de dispositivos actuales
- **Prioridad**: Alta

### RNF-11: Resoluciones de Pantalla
- **Requisito**: Diseño responsive para diferentes tamaños
- **Soportar**: 
  - Teléfonos pequeños (< 5")
  - Teléfonos estándar (5"-6.5")
  - Teléfonos grandes (> 6.5")
- **Notas**: No es necesario soporte para tablets
- **Prioridad**: Alta

### RNF-12: Orientación de Pantalla
- **Requisito**: La aplicación funcionará en modo **vertical (portrait)**
- **Comportamiento**: Bloquear orientación para evitar rotaciones accidentales
- **Prioridad**: Alta

---

## 🌐 Conectividad

### RNF-13: Funcionamiento Sin Internet
- **Requisito**: La aplicación debe funcionar 100% offline
- **Modo Local**: Completamente offline
- **Modo LAN**: Solo requiere WiFi local, no internet
- **Prioridad**: Alta

### RNF-14: Detección de Red WiFi
- **Requisito**: Detectar si hay conexión WiFi para modo LAN
- **Comportamiento**: 
  - Si no hay WiFi, mostrar aviso y sugerir modo local
  - No bloquear la app, solo el modo LAN
- **Prioridad**: Media

### RNF-15: Robustez en Conexión LAN
- **Requisito**: Manejar conexiones inestables gracefully
- **Criterios**:
  - Timeout de conexión: 10 segundos
  - Reintentos automáticos: 3 veces
  - Notificación clara al usuario si falla
- **Prioridad**: Media

---

## 🔒 Seguridad y Privacidad

### RNF-16: Sin Datos Sensibles
- **Requisito**: La app no recolecta ni almacena datos personales
- **Datos locales**: Solo preferencias (volumen, vibración)
- **Prioridad**: Alta

### RNF-17: Comunicación LAN Segura
- **Requisito**: Los datos de juego no deben poder interceptarse fácilmente
- **Implementación**: Protocolo propietario simple, sin datos sensibles
- **Notas**: No requiere encriptación compleja al ser juego casual
- **Prioridad**: Baja

---

## 📦 Mantenibilidad

### RNF-18: Arquitectura Limpia
- **Requisito**: Código siguiendo principios SOLID y patrones Android
- **Arquitectura**: MVVM (Model-View-ViewModel)
- **Criterios**:
  - Separación de UI y lógica de juego
  - Módulos independientes por juego
  - Capa de red separada del resto
- **Prioridad**: Media

### RNF-19: Extensibilidad
- **Requisito**: Fácil añadir nuevos juegos en el futuro
- **Diseño**:
  - Interfaz común para juegos
  - Sistema de plugins/módulos
  - Hub dinámico que lista juegos disponibles
- **Prioridad**: Media

### RNF-20: Versionado
- **Requisito**: Control de versiones con Git
- **Prácticas**: 
  - Commits descriptivos
  - Branches para features
  - Tags para releases
- **Prioridad**: Alta

---

## 🧪 Testeabilidad

### RNF-21: Pruebas Unitarias
- **Requisito**: Tests para lógica crítica de juegos
- **Cobertura objetivo**: > 50% de lógica de negocio
- **Incluir**:
  - Lógica de The Mind (validación de orden)
  - Lógica de El As (intercambios, vidas)
  - Comunicación LAN (mensajes)
- **Prioridad**: Media

### RNF-22: Pruebas de UI
- **Requisito**: Tests automatizados para flujos principales
- **Herramienta**: Espresso
- **Flujos a testear**:
  - Navegación por el hub
  - Inicio de partida local
  - Completar una ronda
- **Prioridad**: Baja

---

## 📊 Resumen de Requisitos No Funcionales

| ID | Categoría | Requisito | Prioridad |
|----|-----------|-----------|-----------|
| RNF-01 | Rendimiento | Carga < 3s | Alta |
| RNF-02 | Rendimiento | Fluidez 30 FPS | Alta |
| RNF-03 | Rendimiento | Latencia LAN < 500ms | Alta |
| RNF-05 | Usabilidad | Interfaz intuitiva | Alta |
| RNF-06 | Usabilidad | Botones grandes | Alta |
| RNF-08 | Usabilidad | Mensajes claros | Alta |
| RNF-10 | Compatibilidad | Android 7.0+ | Alta |
| RNF-11 | Compatibilidad | Multi-resolución | Alta |
| RNF-12 | Compatibilidad | Solo vertical | Alta |
| RNF-13 | Conectividad | 100% offline | Alta |
| RNF-16 | Privacidad | Sin datos personales | Alta |
| RNF-18 | Mantenibilidad | Arquitectura MVVM | Media |
| RNF-19 | Mantenibilidad | Extensible | Media |
| RNF-20 | Mantenibilidad | Git | Alta |

---

## 📝 Notas de Verificación

> **IMPORTANTE**: Estos requisitos deben verificarse durante el desarrollo y especialmente antes de cada entrega.

### Herramientas de Verificación
- **Rendimiento**: Android Profiler
- **Usabilidad**: Tests manuales con usuarios
- **Conectividad**: Pruebas en red real
- **Compatibilidad**: Emuladores con diferentes configuraciones

---

**Última actualización**: 6 de Febrero de 2026
