# DomiSport — Documentación del proyecto

> **TODO EL DEPORTE, EN UN SOLO LUGAR**

Esta carpeta contiene el registro oficial de decisiones, el registro de fuentes y los entregables documentales de PLAN.
No contiene código de producción.

## Estado actual

| Fase | Estado |
|---|---|
| Fase 0 — Descubrimiento, auditoría e investigación (0 → 0.3) | Cerrada |
| Decision Gate | Cerrado y aprobado por el propietario (2026-09-24) |
| PLAN | En curso — PL-01, PL-02, PL-03 y PL-06 cerradas; PL-04 autorizada y en curso, en pausa por E7(c); ninguna otra tarea nueva autorizada (ver [plan/PLAN.md](plan/PLAN.md)) |
| IMPLEMENTATION | **No autorizada** |

Flujo de trabajo (DS-DEC-001):

```text
AUDIT → RESEARCH → DECISION GATE → HUMAN APPROVAL → PLAN → IMPLEMENTATION → TEST → VERIFY → DOCUMENT
```

## Índice

| Documento | Contenido |
|---|---|
| [architecture/principles.md](architecture/principles.md) | Arquitectura central inmutable y requisitos permanentes C16–C23 |
| [decisions/README.md](decisions/README.md) | Formato, vocabulario de estados y reglas del registro de decisiones |
| [decisions/REGISTRY.md](decisions/REGISTRY.md) | **Registro oficial de decisiones (DS-DEC)** |
| [sources/REGISTRY.md](sources/REGISTRY.md) | **Registro oficial de fuentes de datos (DS-SRC)** |
| [plan/PLAN.md](plan/PLAN.md) | Tareas de PLAN, dependencias y criterios de aceptación |

## Reglas que no cambian sin una nueva decisión explícita

- `WEB ≠ PostgreSQL` · `WEB ≠ External Provider` · `WEB → DomiSport API`
- `MLB Stats API = DO_NOT_USE` (DS-DEC-014 / DS-SRC-001)
- Ninguna decisión `PENDING`, `BLOCKED` o `REQUIRES WRITTEN CONFIRMATION` se trata como aprobada.
- Ningún ID de decisión o de fuente se reutiliza.
