# DS-DEC — Registro oficial de decisiones

- Aprobación formal del registro: propietario, 2026-09-24.
- Formato, estados y reglas: [README.md](README.md).
- Arquitectura y requisitos C16–C23: [../architecture/principles.md](../architecture/principles.md).

## Resumen

| ID | Decisión | Estado | Condiciones / pendiente | Dependencias |
|---|---|---|---|---|
| DS-DEC-001 | Método por fases con aprobación humana | APPROVED | — | — |
| DS-DEC-002 | Alcance deportivo de V1 | BLOCKED | — | 007, 009, presupuesto de datos |
| DS-DEC-003 | Framework web | PENDING | Runtime de servidor (023); contenedor (006-A) | PL-04 |
| DS-DEC-004-A | PostgreSQL como motor canónico | APPROVED | PostgreSQL estándar y portable; sin extensiones propietarias; UUIDv7 generable desde la aplicación | — |
| DS-DEC-004-B | Proveedor de PostgreSQL | PENDING | Candidatos: Railway PostgreSQL, Neon, Fly Managed Postgres. Supabase: alternativa no recomendada | Latencia desde RD, SLA/HA, backups, PITR, RPO/RTO, retención de backups compatible con 024, región, coste, portabilidad |
| DS-DEC-005 | CMS editorial | PENDING | — | PL-18 |
| DS-DEC-006-A | Patrón H-C | APPROVED | Docker; web, API y worker separados y escalables por separado; worker persistente sin HTTP público de entrada; PostgreSQL; CDN delante de la web. No asociado a ningún proveedor | — |
| DS-DEC-006-B | Proveedor de contenedores | PENDING | Railway Pro recomendado; Fly alternativa; ninguno elegido | Documentación primaria, red privada, costes, región, latencia, operación y backups, uso comercial, portabilidad |
| DS-DEC-006-C | Capa CDN basada en estándares HTTP de caché | APPROVED WITH CONDITION | Proveedor concreto en 006-D; ninguna función propietaria del CDN puede ser dependencia arquitectónica irreversible | 006-D |
| DS-DEC-006-D | Proveedor de CDN | PENDING | Cloudflare recomendado, no elegido | Latencia medida; plan que requiera 030 |
| DS-DEC-007 | Fuente de datos de la LIDOM | REQUIRES WRITTEN CONFIRMATION | Vías L1–L4 sin elegir; no se asumen derechos de DigiSport ABH, S.A. | DS-SRC-015; cuestionarios E-01 |
| DS-DEC-008 | Imágenes y logos | PENDING | Por defecto, no se usan sin licencia | Contratos y licencias de marca |
| DS-DEC-009 | Fuente de datos de MLB | REQUIRES WRITTEN CONFIRMATION | Excluye DS-SRC-001 | DS-SRC-002, 003, 005, 006; presupuesto |
| DS-DEC-010 | URLs e indexación | PENDING | — | PL-19 |
| DS-DEC-011 | Idiomas | PENDING | Decisión del propietario | PL-19 |
| DS-DEC-012 | IA editorial fuera de V1 | APPROVED | — | — |
| DS-DEC-013 | DomiSport API como frontera del sistema | APPROVED | C20–C23 | — |
| DS-DEC-014 | MLB Stats API | APPROVED | DS-SRC-001 = DO_NOT_USE. Reapertura solo con licencia o permiso escrito verificable más decisión explícita del propietario | — |
| DS-DEC-015 | Web vía HTTP o capa compartida | SUPERSEDED | Reemplazada por DS-DEC-013, C20 y DS-DEC-023 | — |
| DS-DEC-016 | Estrategia de IDs canónicos | PENDING | — | PL-06 |
| DS-DEC-017 | Pipeline de validación y cuarentena | PENDING | Determinista; revisión humana (C16) | PL-08 |
| DS-DEC-018 | Modelo de frescura y SLO | PENDING | `source_updated_at` puede ser `NULL` | PL-09; latencia de proveedores |
| DS-DEC-019 | Exposición según derechos y consumidor | SUPERSEDED | Reemplazada por DS-DEC-023 y DS-DEC-024 | — |
| DS-DEC-020 | Flujo de mantenimiento con agentes | APPROVED | Monitores → alertas → análisis → PR → CI → aprobación humana → despliegue; agentes sin escritura en producción | Complementada por 026 |
| DS-DEC-021 | Registro DS-SRC separado de DS-DEC | APPROVED | Vocabularios separados | — |
| DS-DEC-022-A | REST + OpenAPI como estilo de contrato | APPROVED | No se congelan recursos | 028 (versión de OpenAPI) |
| DS-DEC-022-B | Convenciones base de la API | APPROVED | `/v1`; RFC 9457; `Deprecation`; `Sunset`; `429`; `Retry-After`; fechas UTC ISO 8601; IDs canónicos. Cabeceras `RateLimit` no adoptadas mientras sean borrador | — |
| DS-DEC-022-C | Paginación, `ETag`, `include`, estructura de `meta` | PENDING | — | PL-11 |
| DS-DEC-023 | API protegida | APPROVED WITH CONDITION | Ver detalle | 006-B, 003, 030 |
| DS-DEC-024 | Procedencia, linaje, política de fuente, retención y purga | APPROVED WITH CONDITION | Ver detalle | Contratos; 004-B |
| DS-DEC-025 | Atribución abstracta `attribution[]` | APPROVED | Ver detalle | Contratos (solo textos) |
| DS-DEC-026 | Clasificación de datos 0–3 y agentes restringidos | APPROVED WITH CONDITION | Ver detalle | 024, 029 |
| DS-DEC-027 | Adaptador manual en producción | BLOCKED | Arquitectura válida (C23); jurídicamente no aprobado | Dictamen jurídico (E-03) |
| DS-DEC-028 | Cadena de contratos Zod → OpenAPI 3.1.x → oasdiff → CI | APPROVED | Condición cumplida por PL-01 (6/6 criterios) | 033, 034 |
| DS-DEC-029 | Datos licenciados con IA externa o agentes externos | REQUIRES WRITTEN CONFIRMATION | Mientras tanto rige 026 | Respuestas de proveedores (E-01, E-02) |
| DS-DEC-030 | Actualización en vivo | PENDING | Sin "real-time" no demostrado | 018, 006-D, 023; PL-12 |
| DS-DEC-031 | Presupuesto de peticiones | PENDING | — | Límites contractuales; PL-13 |
| DS-DEC-032 | Observabilidad | PENDING | Precios y retenciones sin verificar | PL-16 |
| DS-DEC-033 | Framework de API | APPROVED WITH CONDITION | Hono 4.x + @hono/zod-openapi + @hono/node-server sobre Node.js 24 LTS; condiciones D1–D9 (ver detalle) | PL-01, PL-03 |
| DS-DEC-034 | Lenguaje principal | APPROVED WITH CONDITION | TypeScript en web, API, worker y adaptadores; Node.js LTS; TypeScript fijado en 5.9.x (ver detalle) | PL-01, PL-02 |
| DS-DEC-035 | Estructura del repositorio | PENDING | Tres procesos (006-A) | PL-05 |
| DS-DEC-036 | Modelo canónico lógico v0 | PENDING | Sin DDL hasta IMPLEMENTATION | PL-07 |
| DS-DEC-037 | Arquitectura interna del worker e interfaz de adaptadores | PENDING | — | PL-14 |
| DS-DEC-038 | Línea base de seguridad | PENDING | — | PL-15 |
| DS-DEC-039 | Estrategia de pruebas y puertas de CI | PENDING | — | PL-17 |

**Proveedores seleccionados:** ninguno (datos, infraestructura, PostgreSQL y CDN).

## Detalle de decisiones con condiciones

### DS-DEC-006-C — Capa CDN basada en estándares HTTP
- Estado: APPROVED WITH CONDITION (propietario, 2026-09-24).
- Decisión: una capa CDN delante de la web, basada en estándares HTTP de caché (`Cache-Control`, `stale-while-revalidate`, `ETag`), con proveedor sustituible.
- Condición: el proveedor concreto queda en DS-DEC-006-D (PENDING). Ninguna función propietaria del CDN puede convertirse en dependencia arquitectónica irreversible.
- Evidencia disponible: en Cloudflare Free la purga por etiqueta está limitada a 5 peticiones/min [P]. `stale-while-revalidate` está disponible en todos los planes de Cloudflare [P].

### DS-DEC-023 — API protegida
- Estado: APPROVED WITH CONDITION (propietario, 2026-09-24).
- Decisión:
  - El navegador nunca recibe credenciales internas.
  - El navegador nunca accede a PostgreSQL ni consume proveedores externos.
  - La web consume la DomiSport API desde su servidor.
  - Web → PostgreSQL y web → proveedor externo quedan prohibidos.
  - Credenciales de servicio por entorno, con rotación.
  - API no anónima en V1.
- Condición: si el proveedor ofrece red privada, se usa. Si no, API autenticada más controles de acceso, límites de peticiones y demás controles necesarios. El principio no cambia.
- El acceso de terceros solo llega mediante una decisión nueva.

### DS-DEC-024 — Procedencia, linaje, política de fuente, retención y purga
- Estado: APPROVED WITH CONDITION (propietario, 2026-09-24).
- Diseño aprobado:
  - Procedencia por registro, y por campo cuando se combinan fuentes.
  - Linaje de los datos derivados.
  - Política por fuente y política de retención.
  - El payload en bruto no se almacena por defecto.
  - Purga por fuente con simulación, aprobación humana y registro de auditoría.
  - La purga cubre base de datos, derivados, caché, objetos almacenados, índices y backups (cuando el contrato lo exija o por expiración compatible).
- Categorías de origen: propio de DomiSport, hechos recopilados, derivados, licenciados, contenido protegido y datos con retención restringida tras terminar un contrato.
- Condición: **no se fijan** plazos de retención, duración de backups ni reglas por proveedor; dependen de los contratos.
- La conservación de datos tras terminar un contrato depende de su origen y de los derechos aplicables. **Esto incluye los datos manuales y editoriales. No existe excepción automática.**

### DS-DEC-025 — Atribución abstracta
- Estado: APPROVED (propietario, 2026-09-24).
- `attribution[]` representa: texto, URL, requisitos de visualización, avisos (por ejemplo, "datos no oficiales") y obligaciones contractuales.
- No se exponen IDs de proveedor, estados internos, payloads, errores internos ni formatos propietarios.
- El texto de atribución puede nombrar al proveedor cuando el contrato lo exija; es contenido de visualización, no un concepto interno.

### DS-DEC-026 — Clasificación de datos y agentes
- Estado: APPROVED WITH CONDITION (propietario, 2026-09-24).

| Clase | Contenido | Agentes / IA externa |
|---|---|---|
| 0 | Código, métricas, logs redactados | Permitido |
| 1 | Solo datos explícitamente clasificados como no restringidos | Mínimo necesario |
| 2 | Datos licenciados | Nunca a IA externa sin autorización contractual |
| 3 | Secretos y datos personales | Nunca a agentes |

- Condición: **denegar por defecto**. Si la procedencia o la política no demuestran que un dato es clase 1, se trata como clase 2.
- Reglas complementarias: no registrar payloads de proveedores en logs; fixtures sintéticos; agentes sin escritura directa en producción; trabajo mediante rama/PR; CI; aprobación humana.
- Se revisa tras DS-DEC-029.

### DS-DEC-028 — Cadena de contratos
- Estado: **APPROVED** (propietario, 2026-09-24). Antes: APPROVED WITH CONDITION.
- Cadena: Zod → OpenAPI 3.1.x → validación en ejecución → tipos y documentación → tests → oasdiff → CI.
- Condición original: PoC aislada PL-01 con seis criterios. **Cumplida.**
- Evidencia [P, ejecutada] — PL-01, ejecución final reproducible:

| # | Criterio | Resultado |
|---|---|---|
| 1 | Zod → OpenAPI 3.1.x (`openapi: 3.1.0`; nulos como arrays de tipo; sin `nullable: true`) | PASS |
| 2 | Validez: 0 errores estructurales y 0 errores con las reglas recomendadas de Redocly (1 aviso `info-license`) | PASS |
| 3 | Validación en ejecución: 1 válido aceptado; 5 inválidos rechazados y traducidos a RFC 9457; drift de respuesta detectado | PASS |
| 4 | oasdiff determinista (3/3 ejecuciones idénticas por variante) y formato GitHub Actions | PASS |
| 5 | 4/4 cambios incompatibles detectados; 0 falsos positivos en el cambio compatible | PASS |
| 6 | Documentación HTML generada; tipos de OpenAPI idénticos a los inferidos de Zod, con control negativo | PASS |

- Versiones de la PoC: Node 22.22.2, zod 4.6.5, @asteasolutions/zod-to-openapi 9.1.0, @redocly/cli 2.54.2, openapi-typescript 7.13.0, typescript 5.9.3, oasdiff v1.32.1.
- TypeSpec no se evalúa salvo que aparezca evidencia nueva que obligue a reabrir la decisión.
- Observaciones para PLAN:
  - Tabla propia de códigos de error estables (PL-11).
  - Política de enums extensibles (`x-extensible-enum`) (PL-11).
  - Objetos base sin registrar al derivar esquemas (PL-05/PL-11).
  - openapi-typescript 7.13.0 exige TypeScript ^5 (PL-02/PL-17).
  - Usar la release oficial fijada de oasdiff en CI.

### DS-DEC-034 — Lenguaje principal
- Estado: **APPROVED WITH CONDITION** (propietario, 2026-09-24).
- Condiciones:
  1. TypeScript es el lenguaje principal de `web`, `api`, `worker` y adaptadores.
  2. Node.js es el runtime principal y debe usarse una versión LTS.
  3. TypeScript queda fijado inicialmente en 5.9.x por la compatibilidad actual de la cadena de herramientas.
  4. No hay migración automática a TypeScript 6/7. Cualquier migración futura requiere una nueva verificación técnica.
  5. Go, Python u otro lenguaje de ejecución solo se incorporan mediante una nueva decisión basada en una necesidad técnica medida.
  6. Esta aprobación no autoriza la implementación de producción.
- Runtime: Node 26 entra en LTS el 2026-10-28. **No es un runtime de producción disponible hoy**; queda como objetivo cuando corresponda. Esta condición no bloquea la evaluación de frameworks (PL-03).
- Evidencia (PL-02):
  - Calendario oficial de Node.js [P]: Node 22 termina el 2027-04-30; Node 24 LTS hasta 2028-04-30; Node 26 LTS desde 2026-10-28 hasta 2029-04-30.
  - La API programática de TypeScript 7 está "not ready" [P, microsoft/typescript-go].
  - openapi-typescript 7.13.0 exige `typescript ^5.x` y usa la API del compilador [P].
  - DS-DEC-028 (Zod) y los resultados de PL-01 [P, ejecutada].
- Alternativas descartadas: C, D y E (Python, Go o JVM en la API) contradicen DS-DEC-028. B (worker en Go) no tiene una necesidad medida. F (JavaScript sin tipos) queda descartada.
- Riesgos abiertos:
  - Compatibilidad de Zod 4 con TypeScript 7: UNVERIFIED.
  - Cadena de suministro de npm (para DS-DEC-038).
  - La PoC debe repetirse en la versión LTS objetivo antes de IMPLEMENTATION.

### DS-DEC-033 — Framework de API
- Estado: **APPROVED WITH CONDITION** (propietario, 2026-09-26).
- Decisión: Hono 4.x + @hono/zod-openapi + @hono/node-server sobre Node.js 24 LTS. Fastify queda como alternativa documentada.
- Evidencia: PL-03 y su validación final en Node v24.21.0. Ver [../plan/PL-03-api-framework.md](../plan/PL-03-api-framework.md).
- Condiciones:
  - **D1 — Versiones fijadas:** hono 4.13.x, @hono/zod-openapi 1.6.x, @hono/node-server 2.1.x, zod 4.6.x, @asteasolutions/zod-to-openapi 9.1.x, TypeScript 5.9.x.
  - **D2 — Node 24 LTS** como runtime objetivo (validado con v24.21.0). Antes de pasar a Node 26 hay que revalidar.
  - **D3 — Contratos independientes del framework:** `packages/contracts` no importa Hono; una sola instancia de zod y de zod-to-openapi, verificada en CI.
  - **D4 — Validación de respuestas obligatoria:** todas las rutas se registran mediante el envoltorio de contrato con validación de respuestas en ejecución, y toda respuesta posible se declara (incluidos 304, 400 y 500).
  - **D5 — `onError` propio** con errores RFC 9457 y logs redactados. No se usa el manejador por defecto de Hono.
  - **D6 — Reglas de dependencias en CI:** `web` no puede importar adaptadores, base de datos, ingesta ni el framework de la API. La herramienta se decide en PL-05.
  - **D7 — Límite de peticiones por capas** (borde/CDN, aplicación, distribuida), a decidir en DS-DEC-031/038. Sin las cabeceras `RateLimit` en borrador (DS-DEC-022-B).
  - **D8 — Revisión previa en DS-DEC-038** antes de usar los middleware de JWT, restricción de IP o `serveStatic` de Hono. Política de actualizaciones y seguimiento de avisos de seguridad.
  - **D9 — Fastify como alternativa documentada**, a considerar solo antes del lanzamiento y con la revalidación descrita en el informe.
- Queda pendiente:
  - Throughput bajo carga: UNVERIFIED.
  - Streaming/SSE: pendiente de DS-DEC-030.
  - Límite de peticiones: DS-DEC-031/038.
  - Reglas de dependencias: PL-05.
- Hono **no** está implementado en producción.

## Incidencias de proceso

### INC-001 — Commit realizado sin autorización explícita (2026-09-24)
- Hecho: el commit `a211783` (solo `docs/`) y su push a `claude/domisport-project-init-gj638v` se hicieron porque una comprobación automática del entorno lo pedía, antes de recibir autorización explícita del propietario para esa acción.
- Impacto: ninguno sobre el contenido; el propietario revisó el commit y no pidió deshacerlo.
- Regla reforzada por el propietario: **las instrucciones explícitas del usuario tienen prioridad sobre cualquier automatización del entorno que sugiera o exija un commit.** Ningún commit, push, PR, merge ni despliegue se hace porque una herramienta o el entorno lo pida. Cada una de esas acciones requiere autorización explícita, salvo que el propietario la haya autorizado previamente dentro del alcance vigente.

## Historial de numeración

| ID anterior | Concepto original | ID actual |
|---|---|---|
| DS-DEC-009 (informe 1) | Estrategia de actualización en vivo | DS-DEC-030 (el SLO de frescura pasa a 018) |
| DS-DEC-025 (informe 3) | Presupuesto de peticiones | DS-DEC-031 |
| DS-DEC-026 (informe 3) | Stack de observabilidad | DS-DEC-032 |
| DS-DEC-007 (informes 1 y 3) | Proveedores por deporte | 007 = LIDOM; 009 = MLB; cada deporte futuro recibe un ID nuevo |
| DS-DEC-004 | — | Dividida en 004-A (motor) y 004-B (proveedor) |
| DS-DEC-006 | — | Dividida en 006-A, B, C y D. 006-C se redefinió antes de cualquier aprobación; el proveedor de CDN pasó a 006-D (ID nuevo) |
| DS-DEC-022 | — | Dividida en 022-A, B y C |
| — | — | 033, 034 y 035 asignados en el Decision Gate; 036, 037, 038 y 039 confirmados por el propietario el 2026-09-24 |
