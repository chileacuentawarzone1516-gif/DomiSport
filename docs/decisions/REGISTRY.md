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
| DS-DEC-028 | Cadena de contratos Zod → OpenAPI 3.1.x → oasdiff → CI | APPROVED WITH CONDITION | PoC PL-01 | 033, 034 |
| DS-DEC-029 | Datos licenciados con IA externa o agentes externos | REQUIRES WRITTEN CONFIRMATION | Mientras tanto rige 026 | Respuestas de proveedores (E-01, E-02) |
| DS-DEC-030 | Actualización en vivo | PENDING | Sin "real-time" no demostrado | 018, 006-D, 023; PL-12 |
| DS-DEC-031 | Presupuesto de peticiones | PENDING | — | Límites contractuales; PL-13 |
| DS-DEC-032 | Observabilidad | PENDING | Precios y retenciones sin verificar | PL-16 |
| DS-DEC-033 | Framework de API | PENDING | — | PL-01, PL-03 |
| DS-DEC-034 | Lenguaje principal | PENDING | — | PL-01, PL-02 |
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
- Estado: APPROVED WITH CONDITION (propietario, 2026-09-24).
- Cadena: Zod → OpenAPI 3.1.x → validación en ejecución → tipos y documentación → tests → oasdiff → CI.
- Condición: la primera tarea de PLAN (PL-01) es una PoC aislada que verifica seis criterios (ver [../plan/PLAN.md](../plan/PLAN.md)).
  - Si falla materialmente un criterio 1–5, se reabre la decisión y se evalúa TypeSpec.
  - Si solo falla el criterio 6, se documenta y se detiene para decisión humana.
- Evidencia documental disponible [P]:
  - `@asteasolutions/zod-to-openapi` ofrece `OpenApiGeneratorV31`.
  - `zod-openapi` soporta 3.1.0 y 3.1.1.
  - oasdiff soporta 3.1 de forma estable desde v1.15.0.

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
