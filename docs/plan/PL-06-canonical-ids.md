# PL-06 — Estrategia de IDs canónicos (DS-DEC-016)

## 0. Estado

- Estado de PL-06: **Cerrada** (2026-09-27).
- **DS-DEC-016: APPROVED WITH CONDITION** (propietario, 2026-09-27). Condiciones en la sección 14 y en el registro de decisiones.
- Tipo de tarea: `Dis + DH`. Autorizaciones del propietario: auditoría, PLAN y documentación de PL-06, y consulta acotada de RFC 9562 (H9 = vía B) (2026-09-26); cierre de PL-06 y decisión sobre DS-DEC-016 (2026-09-27).
- Sin DDL, sin código, sin dependencias, sin datos reales. Los ejemplos son sintéticos.
- Convención de marcas: **CONFIRMADO** (texto aprobado en el repositorio antes de PL-06), **APROBADO** (decisión del propietario, 2026-09-27), **CONDICIÓN** (pendiente registrado como condición de DS-DEC-016), **PENDIENTE**, **INFERENCIA** (valoración de Claude, no evidencia), `UNVERIFIED`.

## 1. Alcance

- **Incluye (APROBADO, H3):**
  - Las entidades canónicas del dominio, tengan o no correspondencia con un ID externo (incluidas las creadas por el adaptador manual, C23).
  - Sus correspondencias con IDs externos de proveedores.
- **Excluye:** los identificadores internos de implementación (consumidores de la API, credenciales, casos de cuarentena, ejecuciones de ingesta, auditoría). Los definen sus propias tareas o decisiones. PL-06 no diseña ninguna política para ellos.
- El catálogo de tipos de entidad es de PL-07. Los tipos que aparezcan aquí son ilustrativos y no vinculantes.

## 2. Restricciones aprobadas (CONFIRMADO)

- `principles.md`: flujo con el paso `EXTERNAL ID → CANONICAL ID` después de la normalización y antes de la detección de anomalías y la cuarentena; el modelo canónico pertenece a DomiSport; los IDs externos se resuelven a IDs canónicos; datos externos = entrada no confiable; detección de anomalías determinista.
- C16: en V1 la cuarentena requiere revisión humana; el propietario revisa los casos ambiguos; Claude analiza y propone, nunca aprueba casos ambiguos.
- C21: la API no expone IDs de proveedores ni estructuras internas del proveedor.
- C23: el adaptador manual es arquitectónicamente válido; su uso en producción depende de DS-DEC-027.
- DS-DEC-004-A (APPROVED): PostgreSQL estándar y portable; sin extensiones propietarias; UUIDv7 generable desde la aplicación. Interpretación en la sección 3.
- DS-DEC-022-B (APPROVED): convenciones base de la API, incluidos los IDs canónicos.
- DS-DEC-024 (APPROVED WITH CONDITION): procedencia por registro y por campo; linaje; purga por fuente; sin plazos de retención fijados; sin excepción automática para datos manuales.
- DS-DEC-025 (APPROVED): `attribution[]` no expone IDs de proveedor.
- DS-DEC-026 (APPROVED WITH CONDITION): clases 0–3 y regla de denegar por defecto.
- DS-DEC-028 y DS-DEC-033 (D3, D4): cadena de contratos Zod → OpenAPI; validación de respuestas.
- Límites de PLAN: sin código, DDL, datos reales ni investigación fuera de las tareas `Inv`.

## 3. H1 — Interpretación de DS-DEC-004-A (CONFIRMADA)

- La interpretación de DS-DEC-004-A es la **(b)**, confirmada por el propietario (2026-09-27): aprueba PostgreSQL estándar y portable, no permite extensiones propietarias y permite generar UUIDv7 desde la aplicación, pero **no** aprueba por sí sola el formato del ID canónico, que corresponde a DS-DEC-016.
- DS-DEC-004-A no se modifica.
- La versión UUID (v4 o v7) queda sujeta a la condición de DS-DEC-016 (sección 6.1).
- Registrada también en DS-DEC-016.

## 4. Terminología

| Término | Significado en este documento |
|---|---|
| ID canónico | Identificador que DomiSport asigna a una entidad canónica del dominio |
| Clave externa | Tupla lógica (fuente, espacio de nombres del proveedor, ID externo) |
| Espacio de nombres | Ámbito de IDs del proveedor, incluido su producto o API cuando tenga varios |
| Correspondencia | Asociación entre una clave externa y un ID canónico, con estado e intervalo |
| Resolución | Paso `EXTERNAL ID → CANONICAL ID` del flujo |
| Sucesor | ID canónico al que apunta un ID fusionado o dividido |

## 5. Alternativas evaluadas

Las ventajas y los costes de estas tablas son valoraciones de Claude (**INFERENCIA**), no evidencia verificada. La alternativa aprobada en cada caso figura en la sección 6.

### 5.1 Alcance (H3)
| Alternativa | Ventajas | Costes / riesgos |
|---|---|---|
| A. Entidades canónicas del dominio y sus correspondencias | Coincide con `principles.md` y DS-DEC-022-B; alcance acotado | Hay que escribir la frontera |
| B. A + identificadores internos | Criterio único | Invade PL-08, PL-10, PL-14, DS-DEC-038 y DS-DEC-040; toca credenciales (clase 3) |

### 5.2 Representación y versión (H4)
| Alternativa | Ventajas | Costes / riesgos |
|---|---|---|
| A. UUID en representación textual estándar | Representación UUID textual estándar; una sola representación. Detalles de formato y compatibilidad contractual pendientes de verificación | No indica el tipo de entidad |
| B. UUID con prefijo de tipo | Legible; evita confundir tipos | Acopla el ID al catálogo de PL-07 (criterio 17); un cambio de tipo cambiaría el ID (criterio 3); no estándar |
| C. Codificación corta en la API (base32/base58) | URLs más cortas | Dos representaciones; códec propio; no estándar |
| D. Cadena opaca en el contrato | Libertad para cambiar la representación | Pierde la validación estándar si no hay patrón |
| Descartadas | Enteros secuenciales (permiten enumerar y exigen coordinación); identificadores derivados determinísticamente de datos externos: descartados porque contradicen el criterio aprobado de independencia respecto de proveedores (criterio 1) | — |

Versión del UUID (v4 frente a v7): ver sección 6.1 y sección 12.

### 5.3 Creación ante un ID externo desconocido (H5)
Familias de política, solo orientativas; su diseño concreto no corresponde a PL-06:

| Familia | Nota |
|---|---|
| Creación automática siempre | Oculta ambigüedades; incompatible con C16 |
| Creación condicionada | Coherente con la regla conceptual de la sección 6.2 |
| Revisión humana siempre | No escala con la llegada continua de eventos (DS-DEC-030) |
| Híbrida | Exigiría política por tipo (PL-07) y reglas de resolución (asignación pendiente) |

### 5.4 Fusión, división y retirada (H6)
| Caso | Alternativas |
|---|---|
| Fusión | (i) el superviviente conserva su ID y el otro pasa a `merged`; (ii) ID nuevo y retirada de ambos |
| División | (i) el original conserva la parte que continúa su identidad; (ii) se retira el original y todas las partes reciben IDs nuevos |
| Retirada | Estado `retired`, o borrado |

### 5.5 Historial de correspondencias (H7)
| Alternativa | Ventajas | Costes / riesgos |
|---|---|---|
| A. Sin historial | Simple | Pierde el linaje (DS-DEC-024) |
| B. Historial con estados e intervalos registrados por DomiSport | Auditable; suficiente para DS-DEC-024 | Algo más complejo |
| C. Bitemporal | Precisión máxima | Exige conocer la validez según el proveedor, que DomiSport normalmente no conoce (`UNVERIFIED`) |

## 6. Estrategia aprobada (APROBADO, con las condiciones de la sección 14)

### 6.1 Formato y representación (H4)
- **APROBADO solo en cuanto a la representación** (propietario, 2026-09-27): UUID textual, **sin prefijo de tipo** y opaco, generado en la aplicación.
- **Opaca** para los consumidores: no deben interpretar su contenido ni ordenar por él.
- La confusión de tipos se evita con las rutas de cada recurso y con tipos internos (implementación posterior).
- **CONDICIÓN (DS-DEC-016):** la versión (v4 o v7) y los detalles de la representación textual quedan pendientes de verificar contra RFC 9562. La verificación se hará después del cierre, dentro del alcance de H9 = vía B, y no bloquea el cierre. Hasta entonces no se elige ninguna versión y sus propiedades siguen `UNVERIFIED` (sección 12); no se usan como argumento.

### 6.2 Creación de IDs (H5)
- **Pueden generar IDs canónicos:** el paso de resolución del flujo de ingesta (incluido el resultado de una revisión humana) y el flujo del adaptador manual (C23).
- **No pueden generarlos:** la web, la API pública ni los adaptadores de proveedor (entregan claves externas opacas, no IDs canónicos).
- **Condiciones conceptuales para crear un ID nuevo:**
  1. Una resolución determinista y no ambigua sin correspondencia existente puede conducir a creación automática. Es una regla **aprobada en PL-06** (H5); no deriva de C16, que no aprueba ninguna política de creación automática.
  2. Un caso ambiguo solo puede terminar en creación por decisión de una revisión humana (C16).
  3. Una entidad creada por un editor mediante el adaptador manual (C23).
  4. Una división aprobada en revisión humana (sección 6.6).
- Un ID canónico nunca se deriva de datos de un proveedor (criterio 1) y ningún agente aprueba su creación en un caso ambiguo.
- Nota de diseño (DS-DEC-031): la resolución no debe exigir consultas adicionales al proveedor por entidad.

### 6.3 Estabilidad y no reutilización
- Un ID canónico es inmutable.
- **No reutilización (invariante):** un ID generado nunca se asigna a otra entidad, en ningún estado, tampoco después de una purga.

### 6.4 Modelo lógico de correspondencia (sin DDL)
- Clave externa = (fuente, espacio de nombres del proveedor, ID externo).
- El ID externo se trata como cadena opaca y se conserva exactamente como llega.
- Cardinalidad: un ID canónico puede tener varias correspondencias activas (varios proveedores, o varios IDs del mismo proveedor si este tiene duplicados).
- Invariante de unicidad de la correspondencia activa: ver sección 6.7.

### 6.5 Estados del intento de resolución
| Estado | Significado |
|---|---|
| `resolved` | Existe una correspondencia activa |
| `created` | Se generó un ID nuevo según la sección 6.2 |
| `ambiguous` | Candidatos o señales contradictorias; requiere revisión humana (C16) |
| `deferred` | Todavía no se puede resolver (por ejemplo, falta la entidad padre o faltan atributos) |
| `rejected` | Una persona decidió no incorporar el registro externo |

Transiciones (criterio 5; APROBADO por el propietario, 2026-09-27):

| Desde | Hacia | Condición |
|---|---|---|
| Llegada de una clave externa | `resolved` | Existe una correspondencia `active` |
| Llegada | `created` | No hay correspondencia y la resolución es determinista y no ambigua (sección 6.2, condición 1) |
| Llegada | `ambiguous` | Candidatos o señales contradictorias |
| Llegada | `deferred` | Todavía no se puede resolver |
| `deferred` | `resolved` / `created` / `ambiguous` / `deferred` | Nuevo intento, con las mismas condiciones que en la llegada |
| `deferred` | `rejected` | Solo por decisión humana |
| `ambiguous` | `resolved` / `created` / `rejected` | Solo en revisión humana (C16); nunca automático ni aprobado por un agente |
| `resolved`, `created`, `rejected` | — | Finales para ese intento; una nueva llegada de la misma clave es un intento nuevo |

- La corrección de una correspondencia ya resuelta pertenece al historial (sección 6.7, `invalidated`), no a estos estados.
- Fuera de PL-06: reglas de emparejamiento y de resolución de entidades (puntuación, umbrales, señales) y tratamiento de los datos que dependen de una entidad ambigua. **Asignación PENDIENTE.** PL-08 es solo una candidata; no se declara responsable. La asignación se decidirá al auditar PL-08. La asignación pendiente no impide el cierre de PL-06 (decisión del propietario, 2026-09-27).

### 6.6 Ciclo de vida del ID canónico (H6)
| Estado | Significado |
|---|---|
| `active` | Vigente |
| `merged` | Fusionado en un sucesor |
| `split` | Dividido en varios sucesores |
| `retired` | Retirado sin sucesor |

- Fusión: alternativa (i). División: (i) si una parte es claramente la continuación; si no, (ii). Retirada: estado `retired`; el borrado solo lo decide la política de purga (PL-10).
- **Grafo de sucesión:**
  - Es acíclico.
  - Los vínculos a sucesores son históricos y no se reescriben.
  - Un sucesor puede cambiar de estado después (por ejemplo, fusionarse otra vez).
  - Recorriendo el grafo siempre se puede determinar el sucesor o los sucesores vigentes (`active`), o que no existe ninguno.
- Un ID no activo conserva su estado y sus vínculos consultables, salvo lo que PL-10 decida para las purgas.
- Fusión y división requieren revisión humana en V1. Es una regla **aprobada en PL-06** (H6), no una extensión de C16.
- Al fusionar, las correspondencias externas se reasignan y queda constancia en el historial (sección 6.7).

### 6.7 Historial de correspondencias (H7, alternativa B)
| Estado | Significado |
|---|---|
| `active` | Vigente |
| `superseded` | Reemplazada por otra (fusión o cambio de ID en el proveedor) |
| `invalidated` | Declarada errónea en revisión humana |
| `retired` | La fuente dejó de aportarla, sin necesidad de purga |
| `purged` | Eliminada según DS-DEC-024; qué queda, si queda algo, lo decide PL-10 |

- Los intervalos representan el **conocimiento y la validez registrados por DomiSport**: desde cuándo y hasta cuándo DomiSport consideró válida la correspondencia. **No** son una afirmación sobre la historia real del proveedor.
- **Invariante de unicidad:** una clave externa tiene como mucho una correspondencia `active` en cada momento.
- Si un proveedor reutiliza un ID para otra entidad, se generan registros distintos, cada uno con su intervalo.
- Cada correspondencia tiene su propia procedencia: fuente, cuándo la registró DomiSport y, si la decidió una persona, quién.
- La conservación del historial depende del contrato (DS-DEC-024). No se promete un historial indefinido.

### 6.8 Exposición en la API
- `/v1` identifica sus recursos con IDs canónicos (DS-DEC-022-B).
- Ningún ID externo, espacio de nombres ni estado de correspondencia aparece en `/v1` (C21); `attribution[]` no se usa para exponerlos (DS-DEC-025).
- Esquemas, códigos HTTP para IDs no activos y alias o redirecciones: PL-11.

### 6.9 Procedencia y purga
- Lo que PL-10 necesita de PL-06: correspondencias con procedencia, purgables por fuente, con historial y estado `purged`.
- Plazos, retención del historial y rastro tras una purga: PL-10.

### 6.10 Clasificación (H8, DS-DEC-026)
| Elemento | Tratamiento |
|---|---|
| ID externo | Se trata como **clase 2** por la regla de denegar por defecto: sin una política que demuestre clase 1, se trata como clase 2. Aplica la regla aprobada; no clasifica su naturaleza |
| ID canónico | **APROBADO** (propietario, 2026-09-27): **clase 1**. Queda clasificado explícitamente como no restringido, como exige la clase 1 de DS-DEC-026, porque no deriva de ningún proveedor (criterio 1) |
| Datos personales | PENDIENTE externo (E-03). El texto de DS-DEC-026 no define "datos personales". No se resuelve en PL-06 |
| Payload y logs | PENDIENTE externo (E-03). El texto no indica si un ID externo es "payload". No se resuelve en PL-06 |

- **INFERENCIA**, no regla: en logs y en los casos que revisen agentes, usar el ID canónico en lugar del ID externo.

### 6.11 Adaptador manual (C23)
- Las entidades creadas mediante el adaptador manual reciben IDs canónicos con las mismas reglas.
- Sus datos no tienen excepción automática de retención (DS-DEC-024).

## 7. Interfaces conceptuales hacia otras tareas

Derivadas de la estrategia aprobada (sección 6). No asignan responsabilidades nuevas a otras tareas.

| Tarea | Recibe de PL-06 | No resuelve PL-06 |
|---|---|---|
| PL-07 | Estrategia de ID aplicable a cualquier tipo de entidad | Catálogo de entidades |
| PL-08 (candidata) | Estado `ambiguous` y regla de revisión humana | Reglas de emparejamiento, cuarentena y flujo de revisión |
| PL-10 | Correspondencias con procedencia, historial y estado `purged` | Política de purga y retención |
| PL-11 | Representación textual y opacidad; estados del ID canónico | Esquemas, códigos HTTP, alias y redirecciones |
| PL-14 | Claves externas opacas | Interfaz de adaptadores |
| PL-19 | Los slugs y las URLs no son IDs | Diseño de URLs |

- Mecanismo de unicidad y coordinación entre réplicas: **sin asignar** (decisión del propietario, 2026-09-27). PL-06 fija solo la invariante de unicidad (sección 6.7).

## 8. Ejemplos sintéticos

Ilustrativos y no normativos. Fuentes ficticias y marcadores sin formato real. No representan ninguna versión de UUID ni ningún proveedor.

| Clave externa | Correspondencia | Estado | ID canónico |
|---|---|---|---|
| (`SRC-SINT-A`, `equipos`, `ext-0001`) | 1 | `active` | `<id-canónico-A>` |
| (`SRC-SINT-B`, `teams`, `T-77`) | 2 | `active` | `<id-canónico-A>` |
| (`SRC-SINT-B`, `teams`, `T-78`) | 3 | `superseded` | `<id-canónico-B>` (`merged` → `<id-canónico-A>`) |
| (`SRC-SINT-B`, `teams`, `T-78`) | 4 | `active` | `<id-canónico-A>` (reasignada tras la fusión) |

## 9. Criterios de aceptación

### 9.1 Criterios funcionales (APROBADOS por el propietario, 2026-09-27)
Rigen PL-06 por decisión del propietario; el criterio 5 incluye las transiciones de la sección 6.5. Los criterios de la transición a PLAN (2026-09-24) no constan en el repositorio.

| # | Verificación | Aprueba si… | Resultado (2026-09-27) |
|---|---|---|---|
| 1 | ID independiente de los proveedores | El ID canónico no se deriva de ningún ID, nombre ni atributo de un proveedor y su validez no depende de ningún proveedor | Cumple (6.2, 5.2) |
| 2 | Creación | Se define qué componentes pueden generar IDs canónicos y cuáles no, y bajo qué condiciones conceptuales se puede crear un ID nuevo | Cumple (6.2) |
| 3 | Estabilidad y no reutilización | Un ID generado es inmutable y nunca se asigna a otra entidad, sea cual sea su estado posterior | Cumple (6.3) |
| 4 | Correspondencia externa | Se definen la clave externa lógica (fuente, espacio de nombres del proveedor e ID externo opaco), su cardinalidad y sus invariantes, sin DDL | Cumple (6.4, 6.7) |
| 5 | Estados de resolución | Se enumeran los estados conceptuales de una resolución y sus transiciones permitidas | Cumple (6.5: estados y transiciones aprobadas) |
| 6 | Ambigüedad (C16) | Ningún caso ambiguo se resuelve automáticamente ni lo aprueba un agente. PL-06 no define reglas de emparejamiento, puntuación, umbrales ni señales | Cumple (6.2, 6.5) |
| 7 | Fusión, división y retirada | Se definen los estados del ID canónico y un grafo de sucesión acíclico que permite determinar el sucesor o los sucesores vigentes, sin diseñar PL-08 ni PL-10 | Cumple (6.6) |
| 8 | Procedencia y purga | Se define qué necesitan PL-10 y DS-DEC-024 de las correspondencias, sin fijar plazos ni la política de purga | Cumple (6.7, 6.9) |
| 9 | C21 / DS-DEC-025 | Ningún ID externo, espacio de nombres ni estado de correspondencia aparece en `/v1`; `attribution[]` no se usa para exponerlos | Cumple (6.8) |
| 10 | DS-DEC-022-B | La API identifica sus recursos con IDs canónicos | Cumple (6.8) |
| 11 | DS-DEC-024 | Las correspondencias tienen procedencia y se pueden purgar por fuente; no se da por supuesta una retención indefinida | Cumple (6.7, 6.9, 6.11) |
| 12 | DS-DEC-026 | Los IDs externos se tratan según la regla de denegar por defecto; los IDs canónicos de DomiSport quedan explícitamente clasificados como clase 1 (aprobado por el propietario el 2026-09-27), coherente con la clase 1 de DS-DEC-026 ("solo datos explícitamente clasificados como no restringidos"); lo que el texto no determina queda pendiente | Cumple (6.10) |
| 13 | C23 | Las entidades del adaptador manual reciben IDs canónicos con las mismas reglas | Cumple (6.2, 6.11) |
| 14 | Exposición en la API | Principios de opacidad, representación y comportamiento de los IDs no activos; esquemas y códigos HTTP quedan para PL-11 | Cumple (6.1, 6.6, 6.8) |
| 15 | Ejemplos | Solo sintéticos: fuentes ficticias e IDs artificiales | Cumple (8) |
| 16 | Sin implementación | No hay DDL, código, dependencias ni datos reales | Cumple (todo el documento) |
| 17 | Independencia de PL-07 | La estrategia vale para cualquier tipo de entidad; la lista de tipos es ilustrativa | Cumple (1, 6) |

### 9.2 Controles M1–M3 (requisitos de cierre documental de PL-06, registrados como condición; no son propiedades funcionales de la estrategia de IDs)
| # | Control | Se cumple si… | Resultado (2026-09-27) |
|---|---|---|---|
| M1 | Etiquetado de la evidencia | Cada afirmación va marcada como confirmado, propuesta o inferencia, con `P`/`P-r`/`S`/`UNVERIFIED` | Cumple (marcas de la sección 0; valoraciones de la sección 5 como INFERENCIA; sección 7 derivada; sección 8 ilustrativa). No se leyeron fuentes externas: no hay etiquetas `P`, `P-r` ni `S`, y lo no verificado figura como `UNVERIFIED` (12) |
| M2 | Compatibilidad con DS-DEC-004-A (b) | Lo que se propone se genera en la aplicación y no necesita extensiones propietarias | Cumple (3, 6.1) |
| M3 | Separación de fases | El entregable no resuelve nada de PL-07, PL-08, PL-10, PL-11 ni PL-14 | Cumple (7, 11; coordinación entre réplicas sin asignar) |

## 10. Dependencias

- Formal (PLAN): DS-DEC-004-A.
- Restricciones de contenido: C16, C21, C23, DS-DEC-022-B, DS-DEC-024, DS-DEC-025, DS-DEC-026, DS-DEC-028 y DS-DEC-033 (D3, D4).
- PL-07 cumple su dependencia de PL-06; su inicio requiere autorización expresa. Su otra dependencia es DS-DEC-024.

## 11. Fuera de alcance

- Catálogo de entidades (PL-07).
- Reglas de emparejamiento y resolución de entidades, cuarentena y flujo de revisión (asignación pendiente; PL-08 candidata).
- Política de purga y retención (PL-10).
- Contrato detallado de la API (PL-11).
- Mecanismo de unicidad y coordinación entre réplicas: sin asignar.
- URLs y slugs (PL-19).
- Identificadores internos de implementación (consumidores, credenciales, cuarentena, ejecuciones de ingesta, auditoría).
- Implementación: librerías, generación concreta de UUID, DDL.

## 12. Evidencia

- **H9 = vía B** (propietario, 2026-09-26): consulta acotada de RFC 9562, limitada a las propiedades de UUIDv4 y UUIDv7 relevantes para H4 (propiedades de cada versión, representación textual estándar y lo necesario para compararlas). No autoriza investigar Node, Zod, PostgreSQL, librerías, benchmarks, rendimiento ni implementaciones.
- **Resultado de la consulta (2026-09-26): RFC 9562 no accesible desde el entorno.** La política de red denegó `www.rfc-editor.org`, `www.ietf.org` y `datatracker.ietf.org`. Se intentó acceder a un borrador del grupo de trabajo y a su repositorio; no se obtuvo ni se usó ningún contenido.
- Por tanto, `UNVERIFIED`:
  - propiedades de UUIDv4;
  - propiedades de UUIDv7 (estructura, orden temporal, información temporal incrustada, monotonía);
  - representación textual estándar según RFC 9562;
  - consideraciones de seguridad y opacidad de RFC 9562.
- Ninguna de esas propiedades se usa como hecho en este documento.

## 13. Riesgos

1. Acoplamiento con PL-07: se mitiga con el criterio 17 y sin prefijos de tipo.
2. Invadir PL-08, PL-10 y PL-11: se mitiga con M3 y con la sección 6.5 limitada a estados e invariantes.
3. Reglas de emparejamiento sin tarea asignada: pueden quedar sin dueño hasta la auditoría de PL-08.
4. Cuello de botella humano (C16) durante los eventos en directo.
5. Versión v4/v7 y detalles de la representación textual sin verificar: condición de DS-DEC-016; se verificarán contra RFC 9562 después del cierre.
6. Clase de los IDs canónicos: resuelta (clase 1, sección 6.10).

## 14. Decisiones del propietario y condiciones

### 14.1 Decisiones adoptadas (propietario, 2026-09-27)
| # | Decisión |
|---|---|
| 1 | Criterios 1–17 aprobados como criterios de PL-06; el criterio 5 incluye las transiciones de la sección 6.5 |
| 2 | M1–M3: requisitos de cierre documental de PL-06, registrados como condición; no son propiedades funcionales de la estrategia de IDs |
| 3 | H1 = (b) confirmada (sección 3) |
| 4 | H3, H5, H6 y H7 aprobadas conforme a este documento |
| 5 | H4 aprobada solo en cuanto a la representación: UUID textual, sin prefijo y opaco |
| 6 | H8: IDs canónicos de DomiSport clasificados como clase 1 |
| 7 | DS-DEC-016: APPROVED WITH CONDITION. PL-06: Cerrada (2026-09-27) |
| 8 | Mecanismo de unicidad y coordinación entre réplicas: sin asignar |
| 9 | La asignación pendiente de las reglas de emparejamiento no impide el cierre de PL-06 |

### 14.2 Condiciones y pendientes
| Tipo | Elemento |
|---|---|
| CONDICIÓN | Versión UUID (v4 o v7) y detalles de la representación textual: verificar contra RFC 9562 después del cierre, dentro del alcance de H9 = vía B. No bloquea el cierre |
| PENDIENTE externo | Datos personales y payload en logs (E-03). No se resuelve en PL-06 |
| PENDIENTE | Asignación de las reglas de emparejamiento y resolución de entidades; se decidirá al auditar PL-08 (solo candidata) |
| SIN ASIGNAR | Mecanismo de unicidad y coordinación entre réplicas |
| Fuera de PL-06 | Emparejamiento, puntuación y umbrales; catálogo, purga y retención; contratos HTTP y URLs; IDs internos; librerías concretas |
