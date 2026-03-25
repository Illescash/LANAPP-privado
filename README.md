# PartyHub

Hub de minijuegos Android para jugar con amigos de forma presencial.
**Asignatura**: Desarrollo de Apps para Dispositivos Moviles - UAM EPS (2025-2026)
**Docente**: Alicia Garrido Pena

> Para contexto tecnico completo (arquitectura, stack, reglas para agentes), ver [`CLAUDE.md`](./CLAUDE.md).

## Juegos

| Juego | Mecanica | Jugadores |
|-------|----------|-----------|
| **The Mind** | Cooperativo: ordenar numeros 1-100 sin comunicarse | 2-8 |
| **El As** | Baraja espanola: intercambiar cartas a ciegas, 3 vidas | 3-8 |

**Modos**: Local (un dispositivo) | LAN (cada uno el suyo, sin internet)

## Documentacion

| Carpeta | Contenido |
|---------|-----------|
| [`00-idea-y-concepto/`](./00-idea-y-concepto/) | Concepto, vision, referencias |
| [`01-requisitos/`](./01-requisitos/) | Funcionales (RF-01..22), no funcionales (RNF-01..22), casos de uso |
| [`02-diseno/`](./02-diseño/) | Arquitectura MVVM, modelo de datos, diseno UI/UX |
| [`03-diseno-ui/`](./03-diseño-ui/) | Mockups (`vistas/`) y prompts de generacion (`prompts/`) |
| [`EL AS/`](./EL%20AS/) | Prototipo web del juego El As |
| [`ENTREGA_1.md`](./ENTREGA_1.md) | Documento Entrega 1 (diagramas Mermaid + prototipos) |

## Entregas

| Entrega | Peso | Contenido | Estado |
|---------|------|-----------|--------|
| E1 | - | Diseno + requisitos | Completada |
| E2 | 30% | Hub + juegos modo LOCAL | Pendiente |
| E3 | 70% | Modo LAN + presentacion | Pendiente |

## Stack

Kotlin, Android Studio, MVVM + Clean, Hilt, Coroutines/Flow, TCP/UDP sockets, Min SDK 24
