# PLAN

- PL-01: completada y aprobada (DS-DEC-028 → APPROVED, 2026-09-24).
- PL-02: completada (DS-DEC-034 → APPROVED WITH CONDITION, 2026-09-24).
- PL-03: cerrada (DS-DEC-033 → APPROVED WITH CONDITION, 2026-09-26).
- Autorizado: ninguna tarea nueva. PL-04 y PL-05 no se inician sin aprobación explícita.
- Commits, push, PR, merges y despliegues: solo con autorización explícita (ver INC-001 en el registro de decisiones).
- IMPLEMENTATION: **no autorizada**.

## Límites

**PLAN puede preparar:** arquitectura interna, contratos (en texto), decisiones pendientes, estructura lógica, criterios de aceptación, estrategia de pruebas, orden de implementación y matriz de dependencias.

**PLAN no puede:**
- Crear código de producción.
- Instalar dependencias en el repositorio.
- Crear esquemas de base de datos, OpenAPI del producto, infraestructura o cuentas.
- Conectar APIs deportivas.
- Crear adaptadores.
- Usar datos reales o fixtures basados en ellos.
- Usar MLB Stats API.

La única excepción es la PoC PL-01, en las condiciones indicadas más abajo.

**Investigación:** solo en tareas marcadas `Inv`, y solo la evidencia necesaria para su criterio de aceptación.

## Tipos de tarea

`Inv` investigación acotada · `PoC` prueba de concepto · `Dis` diseño documental · `DH` decisión humana · `Impl` implementación posterior.

## Tareas

| ID | Objetivo | Resuelve | Depende de | Tipo | Estado |
|---|---|---|---|---|---|
| PL-01 | PoC aislada de la cadena de contratos | Condición de 028; información para 033 y 034 | Autorización (concedida) | PoC + DH | Completada — 6/6 PASS; aprobada |
| PL-02 | Lenguaje principal | 034 | PL-01 | Inv + DH | Entregada — DS-DEC-034 APPROVED WITH CONDITION |
| PL-03 | Framework de API | 033 | PL-01, PL-02 | Inv + PoC + DH | Cerrada — DS-DEC-033 APPROVED WITH CONDITION ([informe](PL-03-api-framework.md)) |
| PL-04 | Framework web (Next.js no se da por supuesto) | 003 | PL-02 | Inv + DH | No iniciada |
| PL-05 | Estructura lógica del repositorio y reglas de dependencia | 035 | PL-02, 03, 04 | Dis + DH | No iniciada |
| PL-06 | Estrategia de IDs canónicos | 016 | 004-A | Dis + DH | No iniciada |
| PL-07 | Modelo canónico lógico v0 (sin DDL) | 036 | PL-06, 024 | Dis + DH | No iniciada |
| PL-08 | Pipeline de validación y cuarentena | 017 | PL-07 | Dis + DH | No iniciada |
| PL-09 | Modelo de frescura y estructura de SLO | 018 | PL-07 | Dis + DH | No iniciada |
| PL-10 | Diseño detallado de procedencia y purga | Condición de 024 | PL-07 | Dis + DH | No iniciada |
| PL-11 | Diseño del contrato /v1, en texto | 022-C | PL-01, 07, 09, 10 | Dis + DH | No iniciada |
| PL-12 | Estrategia de actualización en vivo | 030 | PL-09, 023, 006-C. Cierre condicionado a: 006-D (PL-20), 002 (PL-21), 003 (PL-04), 017 (PL-08), 022-C (PL-11), 031 (PL-13, iterativa), 040 | Dis + DH | No iniciada |
| PL-13 | Modelo de presupuesto de peticiones | 031 | PL-12 (relación iterativa con PL-12, ver DS-DEC-030). Cierre condicionado a: límites contractuales (E-01, E-02), 007/009 (PL-21), E-06 | Dis + DH | No iniciada |
| PL-14 | Arquitectura del worker e interfaz de adaptadores | 037 | PL-08, 13, 006-A | Dis + DH | No iniciada |
| PL-15 | Línea base de seguridad | 038 | PL-05, 11, 14. Cierre condicionado a (proxy de confianza): 006-B y 006-D (PL-20) | Dis + DH | No iniciada |
| PL-16 | Observabilidad | 032 | PL-09, 14 | Inv + Dis + DH | No iniciada |
| PL-17 | Estrategia de pruebas y puertas de CI | 039 | PL-01, 05, 08, 11 | Dis + DH | No iniciada |
| PL-18 | Superficies internas y CMS | 005 | PL-08, 15 | Inv + Dis + DH | No iniciada |
| PL-19 | URLs, indexación, idiomas y logos | 010, 011, 008 | PL-04, 11 | Dis + DH | No iniciada |
| PL-20 | Selección de proveedores de infraestructura | 004-B, 006-B, 006-D | E-04, E-05, PL-10 | Inv + DH | Bloqueada |
| PL-21 | Decisiones de datos | 007, 009, 029, 027 y después 002 | E-01, E-02, E-03, E-06 | DH | Bloqueada |
| PL-22 | Orden de implementación y matriz de dependencias | — | PL-01 a PL-19 | Dis + DH | No iniciada |
| PL-23 | Revisión de cierre de PLAN | — | PL-22 | DH | No iniciada |

"Depende de" indica lo necesario para iniciar una tarea. Una tarea puede iniciarse y no poder cerrarse mientras una decisión de la que depende su cierre siga pendiente o bloqueada. Esas dependencias se indican como "Cierre condicionado a" y no cambian el estado de la tarea.

Los criterios de aceptación de cada tarea son los aprobados en la transición a PLAN (2026-09-24). Se desarrollarán en este documento al ejecutar cada tarea.

## Tareas externas del propietario

| ID | Acción | Desbloquea |
|---|---|---|
| E-01 | Cuestionario a LIDOM y DigiSport ABH | 007. Aporta evidencia a 031 y 018 solo si el cuestionario incluye cuotas, frecuencia de actualización, latencia y modalidad de entrega (contenido no registrado en `docs/`: UNVERIFIED) |
| E-02 | Cuestionarios a Sportradar, SportsDataIO, MySportsFeeds y BALLDONTLIE | 009, 029. Aporta evidencia a 031 y 018 solo si el cuestionario incluye cuotas, frecuencia de actualización, latencia y modalidad de entrega (contenido no registrado en `docs/`: UNVERIFIED) |
| E-03 | Consulta a un abogado dominicano | 027 y la vía L3 de 007 |
| E-04 | Medir la latencia desde un equipo en la RD (crear cuentas requiere autorización aparte) | 004-B, 006-B, 006-D |
| E-05 | Ampliar el acceso de red del entorno | PL-20; lectura del *Winter League Agreement* |
| E-06 | Presupuesto de licencias de datos | 002, 009 |
| E-07 | Autorizar `docs/` | **Completada** (2026-09-24) |

## Ruta crítica

PL-01 → PL-02 → (PL-03 ∥ PL-04) → PL-05; en paralelo, PL-06 → PL-07 → (PL-08 ∥ PL-09 ∥ PL-10) → PL-11 → PL-12 → PL-13 → PL-14 → PL-15 → PL-16 → PL-17 → PL-18 → PL-19 → PL-22 → PL-23.

PL-20 y PL-21 van en carril propio y dependen de las tareas E. PLAN puede cerrarse con ambas bloqueadas si PL-22 lo refleja.

## PL-01 — Especificación

**Condiciones de la excepción:**
- Fuera del repositorio, en el directorio temporal de la sesión.
- Sin commits; sin modificar el repositorio; sin ficheros en `docs/` como parte de la PoC.
- Solo esquemas y ejemplos sintéticos. Sin datos reales, propietarios ni licenciados.
- Sin APIs deportivas, sin MLB Stats API, sin adaptadores, sin infraestructura y sin credenciales.
- Solo las dependencias estrictamente necesarias, con versiones fijadas y registradas.
- Resultado como informe en el chat.

**Criterios de aceptación:**

| # | Verificación | Aprueba si… |
|---|---|---|
| 1 | Zod → OpenAPI 3.1.x | Se genera `openapi: 3.1.x` sin edición manual, con nulos expresados como arrays de tipo (JSON Schema 2020-12) |
| 2 | Validez | El validador devuelve 0 errores |
| 3 | Validación en ejecución | 1 ejemplo válido aceptado; 5 inválidos rechazados con error estructurado y traducibles a RFC 9457; se detecta una respuesta que no cumple el esquema |
| 4 | Integración de oasdiff | Se ejecuta sobre dos especificaciones generadas con códigos de salida deterministas, en modo CI |
| 5 | Cambio incompatible | 100 % de 4 cambios incompatibles detectados (eliminar campo, cambiar tipo, volver obligatorio un parámetro opcional, renombrar un valor de enum) y 0 falsos positivos con un campo opcional añadido |
| 6 | Documentación y tipos | Documentación generada sin errores; tipos generados desde OpenAPI coherentes con los inferidos de Zod en una prueba de compilación |

**Resultado:**
- Fallo material de 1–5: se reabre DS-DEC-028 y se evalúa TypeSpec.
- Solo falla 6: se documenta y se detiene para decisión humana.
