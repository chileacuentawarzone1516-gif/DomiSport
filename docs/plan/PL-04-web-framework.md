# PL-04 — Framework web (DS-DEC-003)

## 0. Estado

- Estado de PL-04: **En curso — RESEARCH documental; en pausa por E7(c)**. PL-04 **no está cerrada**.
- **DS-DEC-003: `PENDING`** (sin cambios). Ningún framework está seleccionado.
- Filtro documental E1–E8: **0 candidatos lo superan actualmente** (sección 22).
- Tipo de tarea: `Inv + DH`.
- Autorizaciones del propietario (todas el 2026-09-27):
  - AUDIT de PL-04 (solo lectura).
  - PLAN de PL-04, aprobado con los parámetros 1–15 y las correcciones posteriores (secciones 5–12).
  - RESEARCH documental 1 con el mecanismo M-P y la lista de dominios y repositorios de la sección 5.
  - RESEARCH documental 2, limitada a E1–E8 de seis candidatos.
  - Fase DOCUMENT (este informe).
- **No autorizado y no realizado:** PoC, fase comparativa C1–C11, selección de framework, cierre de PL-04, `access:"push"` en GitHub, excepción del parámetro 7, evaluación de Angular 21.
- Fechas en `America/Santo_Domingo` (UTC−4), según la convención de fechas de [../decisions/README.md](../decisions/README.md).
- Marcas de este documento:
  - estados de criterio: **C** Cumple · **P** Parcial · **NC** No cumple · **U** `UNVERIFIED`;
  - etiquetas de evidencia: `P`, `P-r`, `S`, `UNVERIFIED` (sección 8);
  - **INFERENCIA**: valoración de Claude, no evidencia.
- La investigación la realizó Claude. Las decisiones y ratificaciones son del propietario.

## 1. Objetivo

Obtener la evidencia documental necesaria para que el propietario decida DS-DEC-003 (framework web). Para ello, los candidatos se evalúan contra criterios eliminatorios (E1–E8) y comparativos (C1–C11) congelados antes de ver resultados. Una recomendación de Claude nunca equivale a una aprobación (Reglas 1 y 2 del registro de decisiones).

## 2. Alcance

- Investigación documental de los candidatos de la sección 13, limitada a la evidencia necesaria para E1–E8 y C1–C11.
- Filtro documental E1–E8 y, para los candidatos que lo superen, comparación C1–C11 y selección de un máximo de 3 para la PoC (sección 11).
- PoC con datos sintéticos, con autorización aparte (sección 12).
- Informe con la evidencia y el estado de la investigación. La decisión de DS-DEC-003 corresponde al propietario.

## 3. Fuera de alcance

- Seleccionar framework, CDN, plataforma o estrategia de caché. La implementación.
- CDN y plataforma (DS-DEC-006-D, PL-20); directivas y estrategia de caché (DS-DEC-030, PL-12).
- URLs e indexación como estrategia (PL-19); CMS (PL-18).
- Estructura del repositorio y herramienta de D6 (PL-05); línea base de seguridad (PL-15); stack de observabilidad (PL-16).
- Contrato de los endpoints de la web: PL-04 no lo define ni lo aprueba (parámetro 12).
- Proxy de confianza y política de límite de peticiones (DS-DEC-038, DS-DEC-040; parámetro 13).
- Sistema de diseño, costes y benchmarks absolutos.
- Licencias de dependencias transitivas no indispensables y cualquier revisión jurídica exhaustiva.
- Datos reales, APIs deportivas y MLB Stats API.
- Angular 21: no se evalúa sin autorización expresa y separada del propietario.
- Operaciones de Git de escritura, salvo autorización expresa.

## 4. Requisitos

### 4.1 Derivados de decisiones aprobadas (vinculantes)

| ID | Requisito | Fuente |
|---|---|---|
| R1 | La web consume la DomiSport API por HTTP desde su servidor; nunca accede a PostgreSQL ni a proveedores | DS-DEC-013, C20, DS-DEC-023 |
| R2 | Las credenciales de servicio solo existen en el servidor web; el navegador nunca las recibe | DS-DEC-023 |
| R3 | Proceso web separado, en contenedor Docker y escalable por separado | DS-DEC-006-A |
| R4 | CDN delante de la web con caché HTTP estándar, sin dependencia irreversible de funciones propietarias; proveedor sin elegir | DS-DEC-006-C, DS-DEC-006-D |
| R5 | TypeScript 5.9.x y runtime Node.js LTS. La versión concreta para la web la fija DS-DEC-003 (sección 4.4) | DS-DEC-034 |
| R6 | La web no importa adaptadores, base de datos, ingesta ni Hono | DS-DEC-033 (D6) |
| R7 | La web consume los contratos compartidos (`packages/contracts`) y los tipos de la cadena Zod → OpenAPI | DS-DEC-033 (D3), DS-DEC-028, [PL-03](PL-03-api-framework.md) |
| R8 | La web trata los IDs canónicos como opacos | DS-DEC-016 |

### 4.2 Derivados de decisiones propuestas (condicionales; no se deciden aquí)

| ID | Requisito | Fuente |
|---|---|---|
| R9 | Sondeo desde el navegador a endpoints de la web cacheados en el CDN. PL-04 solo comprueba la capacidad técnica | DS-DEC-030 (`PROPOSED`) |
| R10 | Límite de peticiones en el servidor web por IP del cliente detrás de un proxy de confianza. PL-04 solo comprueba que se puede leer una cabecera configurable | DS-DEC-040 (`PROPOSED`); DS-DEC-038 (`PENDING`) |
| R11 | SSE: solo capacidad futura, informativa | DS-DEC-030, condición 9 |

### 4.3 Pendientes de otras tareas (no se inventan)

- Indexación y URLs (DS-DEC-010, PL-19), idiomas (DS-DEC-011, PL-19), logos (DS-DEC-008), CMS (DS-DEC-005, PL-18), antigüedad del dato (DS-DEC-018 y punto 3 de DS-DEC-030) e inventario de páginas (DS-DEC-002, `BLOCKED`).
- PL-04 los trata como capacidades a comprobar. PL-19 es una tarea posterior que depende de PL-04, no al revés.
- No se añaden dependencias formales nuevas de PL-04 sobre PL-19, PL-15, PL-16, PL-18, PL-20 ni PL-12 (parámetro 15).

### 4.4 Runtime

- Node 24 LTS es el entorno **provisional y homogéneo** de la PoC, porque es el runtime validado de la API (DS-DEC-033, D2). **No fija el runtime de la web**, que se decide en DS-DEC-003 (parámetro 10).
- Node 26 entra en LTS el 2026-10-28 (DS-DEC-034).

### 4.5 No funcionales

Rendimiento (sección 12.3), accesibilidad (sección 12.4), observabilidad (DS-DEC-032, PL-16) y seguridad (DS-DEC-038, PL-15). PL-04 comprueba que el framework **permite** cumplirlos. No fija umbrales de producto que no estén aprobados.

## 5. Metodología de investigación

### 5.1 Qué se investiga

Solo lo necesario para cada criterio:
- licencia (E5);
- política de soporte, versiones, estado del repositorio y canal de seguridad (E7);
- compatibilidad con Node LTS y TypeScript 5.9.x (E1, E2);
- límite entre servidor y cliente para los secretos (E3);
- ejecución como servidor Node autónomo en contenedor sin plataforma propietaria (E2, E4);
- endpoints propios y control de cabeceras de respuesta (E8);
- consumo de un paquete TypeScript compartido en un monorepo (E6);
- modos de renderizado (C1), indexación (C2) e idiomas (C3);
- mecanismos de observabilidad (C8) y conexiones de larga duración (C11, informativo).

### 5.2 Mecanismo M-P (aprobado por el propietario)

- `curl` solo con GET y HTTPS, sin credenciales ni tokens, sin `-L`, con verificación TLS activa y con `--fail`.
- El cuerpo y las cabeceras se guardan temporalmente **fuera del repositorio**.
- Se comprueban: código de salida, HTTP 200, `Content-Length` cuando existe, SHA-256 del contenido, `ETag` y `Last-Modified` cuando existen, y fecha y hora en `America/Santo_Domingo`.
- **Lectura completa** del contenido recibido:
  - en HTML, el texto se extrae de forma determinista (sin scripts ni estilos y sin resumir) y se lee entero;
  - en JSON de `registry.npmjs.org` y `api.github.com`, "leída completa" significa descarga íntegra, verificación de integridad, análisis determinista del documento completo y extracción de los campos pertinentes. Esto no convierte los datos en suficientes por sí mismos: siguen sujetos a E1–E8 y a la definición de `P`.
- **WebFetch no sirve como evidencia `P`**, porque devuelve la respuesta de un modelo, no el contenido original.
- Uso limitado a la evidencia de E1–E8 y C1–C11, sin navegación exploratoria. Sin buscadores ni agregadores.
- Leer el registro de paquetes no es instalar. No se instaló ningún paquete.

### 5.3 Redirecciones

- "Mismo dominio" significa **host exacto**.
- Una redirección al mismo host autorizado se puede seguir. Una redirección a otro host no se sigue automáticamente, y a un host no autorizado nunca se sigue.
- Se registran la URL original, el código 3xx y `Location`. Si no se puede seguir, la evidencia afectada queda en `UNVERIFIED`.

### 5.4 Dominios y repositorios autorizados

- **Dominios núcleo:** `nextjs.org`, `reactrouter.com`, `docs.astro.build`, `svelte.dev`, `nuxt.com`, `angular.dev`, `docs.solidjs.com`, `nodejs.org`, `www.w3.org`, `github.com`, `registry.npmjs.org`.
- **`raw.githubusercontent.com`:** solo para archivos definidos (`LICENSE`, `SECURITY.md`, política de soporte en forma de archivo, archivos de cambios y de seguridad) y solo cuando hacen falta.
- **`api.github.com`:** solo para los endpoints definidos (repositorio, versiones, avisos de seguridad y commits) y solo cuando hacen falta.
- **Repositorios de terceros, solo lectura:** `vercel/next.js`, `remix-run/react-router`, `withastro/astro`, `sveltejs/kit`, `nuxt/nuxt`, `angular/angular`, `angular/angular-cli`, `solidjs/solid-start`, `nodejs/node`, `nodejs/Release`, `dequelabs/axe-core`. Sin clonar y sin escribir.
- **Leer el paquete publicado `@solidjs/start@2.0.5`** en `registry.npmjs.org` sin instalarlo (autorizado para E5 en la RESEARCH 2).
- **No autorizados:**
  - dominios condicionales: `nitro.build`, `vite.dev`, `react.dev`, `vuejs.org`, `spdx.org`, `opensource.org`, `vercel.com`, `kit.svelte.dev`, `start.solidjs.com`, `angular.io`;
  - repositorios `vercel/.github` y `sveltejs/.github`;
  - `www.npmjs.com` (no necesario).
- Si un dominio autorizado está bloqueado por el entorno, se registra el bloqueo, la evidencia queda en `UNVERIFIED` y no se reintenta ni se rodea.

### 5.5 Versión evaluada (ratificada)

- **Versión evaluada = dist-tag `latest` en `registry.npmjs.org` a la fecha de evaluación.**
- Fue un criterio operativo aplicado de forma uniforme y el propietario lo ratificó el 2026-09-27.
- No se reevalúan versiones anteriores sin autorización explícita.
- Fecha de evaluación: 2026-09-27. La ventana de E7(b) va del 2025-09-27 al 2026-09-27.

## 6. Criterios eliminatorios E1–E8

Un **No cumple** descarta al candidato. El tratamiento de Parcial y `UNVERIFIED` está en la sección 11.

| ID | Criterio |
|---|---|
| E1 | Una aplicación escrita en TypeScript 5.9.x puede usar el framework con compatibilidad y soporte oficial documentados. Se evalúa la aplicación, no la versión de TypeScript con la que está implementado internamente el framework |
| E2 | Se ejecuta como servidor Node LTS autónomo en contenedor |
| E3 | Tiene un límite documentado entre servidor y cliente que impide enviar secretos al navegador |
| E4 | No exige una plataforma ni un CDN propietarios para funcionar |
| E5 | Licencia compatible con uso comercial (sección 9) |
| E6 | Puede consumir los contratos compartidos sin importar Hono, la base de datos ni los adaptadores |
| E7 | Mantenimiento activo (sección 10) |
| E8 | **Capacidad técnica:** permite definir endpoints HTTP propios y fijar por ruta las cabeceras de respuesta, incluidas las de caché y las de validación condicional. Se basa en DS-DEC-006-C y en R9. **No incluye** ni el contrato de esos endpoints ni las directivas de caché |

Separación respecto de DS-DEC-030 (parámetro 12):

| Aspecto | ¿Lo resuelve PL-04? |
|---|---|
| Capacidad del framework para crear endpoints y controlar cabeceras | Sí, se evalúa (E8, C9, V3) |
| Contrato y validación de los endpoints de la web | No. PL-04 no lo define ni lo aprueba |
| Directivas concretas de caché | No (PL-12, DS-DEC-006-D). Los valores usados en la PoC son solo de prueba |
| Decisión de DS-DEC-030 | No. Sigue en `PROPOSED` |

## 7. Criterios comparativos C1–C11 y prioridades

| ID | Criterio | Nivel |
|---|---|---|
| C1 | Flexibilidad de renderizado (SSR, SSG, CSR) | N1 |
| C2 | Capacidad de indexación | N2 |
| C3 | Idiomas: solo capacidad técnica; no decide idiomas ni alcance de producto (parámetro 11) | N2 |
| C4 | Rendimiento comparativo (sección 12.3). Solo se decide con la PoC | N2 |
| C5 | Apoyo a la accesibilidad (sección 12.4) | N2 |
| C6 | Mantenibilidad: complejidad y política de actualizaciones | N1 |
| C7 | Historial y política de seguridad. La evidencia decisiva sale del canal oficial del proyecto (sección 8) | N1 |
| C8 | Mecanismos de observabilidad | N3 |
| C9 | Granularidad del control de cabeceras | N1 |
| C10 | Facilidad para probar | N3 |
| C11 | Conexiones de larga duración | Informativo |

- Niveles aprobados: **N1** C1, C6, C7, C9 · **N2** C2, C3, C4, C5 · **N3** C8, C10 · C11 informativo.
- Sin puntuaciones numéricas. La comparación usa la matriz por niveles.
- C1–C11 solo se aplica a los candidatos que superen el filtro E1–E8. **En PL-04 aún no se ha ejecutado** (sección 23).

## 8. Estados y reglas de evidencia

### 8.1 Etiquetas de evidencia

Texto literal de [../decisions/README.md](../decisions/README.md), sin cambios:

| Etiqueta | Significado |
|---|---|
| `P` | Fuente primaria leída completa (contrato, términos, documentación o repositorio oficial). |
| `P-r` | Fuente primaria localizada mediante buscador, no leída completa. |
| `S` | Fuente secundaria. |
| `UNVERIFIED` | No confirmado. |

"Nunca se convierte `P-r` en `P`, `S` en `P`, ni una hipótesis en un hecho."

### 8.2 Estados de un criterio

| Estado | Cuándo se asigna |
|---|---|
| Cumple | Una fuente `P` o la PoC demuestran el criterio completo, sin depender de nada prohibido |
| Parcial | Una fuente `P` o la PoC demuestran que se cumple con una limitación documentada |
| No cumple | Una fuente `P` o la PoC demuestran que no es posible, o que solo lo es violando R1–R8 o E4 |
| `UNVERIFIED` | No hay fuente primaria accesible, solo hay `P-r` o `S`, o las fuentes primarias se contradicen sin resolverse |

### 8.3 Reglas

- Cada estado cita su evidencia: URL, fecha y versión evaluada.
- `P-r` o `S` por sí solas no pueden producir Cumple, Parcial ni No cumple. Un criterio cuya única evidencia sea `P-r`, `S` o `UNVERIFIED` queda en `UNVERIFIED`.
- Una fuente primaria que no se ha leído completa queda en `UNVERIFIED`.
- Que la documentación oficial no diga nada sobre una capacidad da `UNVERIFIED`, no No cumple. La excepción es E7(a)/(d), según la sección 10.
- **`UNVERIFIED` por bloqueo de acceso no equivale a No cumple.**
- **GitHub Advisory Database es fuente `S`**: solo sirve para corroborar o dar contexto. La evidencia decisiva de E7(d) y C7 viene del canal oficial del proyecto: `SECURITY.md`, la página Security del repositorio (política y avisos publicados por el proyecto), los avisos oficiales del proyecto o una documentación oficial equivalente.
- **Fuentes de `raw.githubusercontent.com` por rama** (`/main/`, `/canary/`): son `P` si se descargaron y leyeron completas, pero **no están fijadas a un SHA de commit**. El SHA-256 registrado es un hash del contenido, no un SHA de commit Git.
- Información solo de memoria: nunca es evidencia.
- **Congelación:** criterios, estados, niveles y reglas quedaron fijados antes de ver resultados. Solo cambian por decisión del propietario antes de conocer resultados. Un cambio posterior obliga a reevaluar todos los candidatos y a registrarlo.

## 9. E5: licencia, perímetro y agregación

- **Fuente:** licencia oficial identificable en el repositorio oficial o en los metadatos del paquete oficial.
- **Cumple:** MIT, Apache-2.0, BSD-2-Clause, BSD-3-Clause o ISC (lista congelada).
- **Parcial:** cualquier otra licencia compatible. Requiere revisión expresa del propietario de cada paquete afectado.
- **No cumple:** prohíbe o restringe el uso comercial, es de código disponible con restricciones de uso o exige pagos por uso comercial.
- **`UNVERIFIED`:** no se identifica en una fuente oficial, o el repositorio y los metadatos se contradicen.
- **Perímetro:**
  1. el núcleo del framework;
  2. sus paquetes propios indispensables (por ejemplo, el adaptador o servidor Node oficial);
  3. las bibliotecas base que la aplicación instala directamente y que son indispensables para renderizar o ejecutar el candidato: `react`/`react-dom`, `vue`, `svelte`, `solid-js` y los `@angular/*` indispensables.
- Quedan fuera las dependencias transitivas no indispensables. **E5 no es una auditoría jurídica** ni una revisión exhaustiva de dependencias transitivas.
- **Agregación:** el E5 del candidato es el estado más restrictivo entre los paquetes de su perímetro, en el orden **No cumple > `UNVERIFIED` > Parcial > Cumple**. No modifica los estados individuales ni la regla de revisión expresa del Parcial.

## 10. E7: mantenimiento activo

- **Cumple**, si se dan las cuatro condiciones:
  - (a) hay una política de soporte o una lista de versiones soportadas publicada oficialmente;
  - (b) hay al menos una versión estable en los 12 meses anteriores a la fecha de evaluación;
  - (c) el repositorio oficial no está archivado ni declarado obsoleto;
  - (d) existe un canal oficial de seguridad del proyecto, acreditado solo con las fuentes oficiales de la sección 8.3.
- **Parcial:** se cumplen (b) y (c), pero las fuentes oficiales consultadas no muestran (a) o (d).
- **No cumple:** repositorio archivado u obsoleto, ninguna versión estable en la ventana, o fin de vida anunciado de la versión evaluada sin sucesora.
- **`UNVERIFIED`:** fuentes oficiales inaccesibles. Las fuentes secundarias no las sustituyen.
- Se registra la fecha de evaluación.

## 11. Reglas de filtro y selección

1. Se parte de la lista inicial aprobada (sección 13).
2. Filtro documental con E1–E8, candidato por candidato:
   - No cumple en cualquier eliminatorio: descartado.
   - `UNVERIFIED` en cualquier eliminatorio: no supera el filtro. Solo puede pasar a la PoC con autorización expresa del propietario (parámetro 7).
   - Parcial en E1–E4 o E6–E8: no supera el filtro. Solo puede pasar a la PoC con autorización expresa del propietario.
   - Parcial en E5: requiere revisión expresa del propietario, que decide si E5 supera el filtro.
3. Resultado del filtro:
   - si lo superan 3 o menos, pasan todos a la PoC;
   - si lo superan más de 3, se reducen a 3 comparando N1, luego N2 y luego N3 con la evidencia documental;
   - los empates los decide el propietario;
   - si no lo supera ninguno, se detiene el proceso y se informa al propietario.
4. Los candidatos que entran por autorización expresa cuentan dentro del máximo de 3.
5. La referencia de control entra en la PoC fuera del máximo de 3, solo como línea base técnica y comparativa (parámetro 9).

## 12. PoC: metodología (no ejecutada)

La PoC necesita autorización aparte. **No está autorizada y no se ha ejecutado.**

### 12.1 Condiciones

Fuera del repositorio, datos sintéticos, API `/v1` simulada, sin proveedores reales ni MLB Stats API, versiones fijadas y registradas, sin commits, Node 24 LTS provisional (sección 4.4).

### 12.2 Pruebas

| # | Prueba | Criterios |
|---|---|---|
| V1 | Página renderizada en servidor que consume la API simulada con una credencial de servicio. La credencial no aparece en el paquete del cliente ni en las respuestas | R1, R2, E3 |
| V2 | Página estática | C1 |
| V3 | Endpoint sondeable con cabeceras de caché y de validación condicional configurables, con valores de prueba | E8, C9 |
| V4 | Importar un `packages/contracts` sintético y comprobar que no se resuelven Hono, la base de datos ni los adaptadores | R6, R7, E6 |
| V5 | Construir y ejecutar en Docker | R3, E2 |
| V6 | Middleware que lee la IP del cliente desde una cabecera de confianza configurable. Solo capacidad técnica: no elige proxy de confianza ni política de límite de peticiones (parámetro 13) | R10 |
| V7 | Enrutado por idioma. Se ejecuta solo como capacidad técnica (parámetro 11) | C3 |
| V8 | Accesibilidad (sección 12.4) | C5, AC-9 |
| V9 | Rendimiento (sección 12.3) | C4, AC-8 |
| V10 | Los errores no exponen información del proveedor y los logs son estructurados. El manejo de errores (C7) se separa conceptualmente del logging y la observabilidad (C8), aunque se prueben juntos (parámetro 14) | C7, C8 |

### 12.3 Rendimiento (comparativo y orientativo, no un benchmark absoluto)

- Misma máquina, aplicación equivalente, misma API simulada con latencia fija y mismas condiciones: construcción de producción, Node 24 LTS, versiones fijadas y herramienta de carga fijada.
- Procedimiento: 5 peticiones de calentamiento y 30 secuenciales por página. Se registran la mediana y el p90 del tiempo de primera respuesta.
- También se registran el JavaScript enviado al cliente comprimido, el tamaño de la construcción y de la imagen del contenedor, y la memoria en reposo.
- Sin umbrales de producto aprobados: AC-8 solo registra una línea base.

### 12.4 Accesibilidad

- Referencia: WCAG 2.2 nivel AA. La misma página de ejemplo en todos los candidatos.
- Herramienta automática con versión fijada, por confirmar.
- Lista manual acotada: navegación con teclado y foco visible, `lang` en el documento, estructura de encabezados y regiones, textos alternativos y gestión del foco al navegar en el cliente.
- Las herramientas automáticas detectan solo parte de los problemas y no demuestran la conformidad. PL-04 no es una auditoría completa de accesibilidad.

### 12.5 AC-9 (aprobado con corrección, parámetro 1)

AC-9 evalúa **la capacidad del framework para permitir una implementación accesible**, no la accesibilidad universal.
- La PoC no debe producir violaciones críticas en las comprobaciones automatizadas definidas.
- Debe demostrar la capacidad de implementar los controles manuales establecidos.
- Se documentan las limitaciones de las herramientas automáticas.

### 12.6 Criterios de aceptación (aprobados)

| # | Área | Se cumple si… |
|---|---|---|
| AC-1 | Integración con la API | V1 superada |
| AC-2 | Arquitectura API-first | El framework elegido no requiere acceso directo a datos (R1), con evidencia en la matriz |
| AC-3 | Separación de la base de datos y los proveedores | V4 superada |
| AC-4 | TypeScript | E1 en Cumple; compila en modo estricto con TypeScript 5.9.x |
| AC-5 | Node LTS | V5 superada en Node 24 LTS provisional; la versión final del runtime la fija DS-DEC-003 |
| AC-6 | Renderizado | Modos de renderizado documentados (V1, V2) y su encaje con los requisitos confirmados |
| AC-7 | Indexación | Capacidad documentada (C2), sin fijar decisiones de PL-19 |
| AC-8 | Rendimiento | Métricas de la sección 12.3 registradas; línea base, salvo umbrales aprobados |
| AC-9 | Accesibilidad | Según la sección 12.5 |
| AC-10 | Idiomas | Capacidad técnica verificada (V7) |
| AC-11 | Contenedor | V5 superada |
| AC-12 | Independencia del proveedor y del CDN | E4 en Cumple |
| AC-13 | Actualización en vivo | E8 y V3 superados como capacidad, sin seleccionar estrategia de caché |
| AC-14 | Mantenibilidad | E7 en Cumple y política de versiones documentada |
| AC-15 | Seguridad | V1 y V10 superados; política de avisos documentada; la revisión de middleware queda para DS-DEC-038 |
| AC-16 | Observabilidad | C8 documentado, sin elegir stack |
| AC-17 | Contratos OpenAPI y Zod | V4 superada |
| AC-18 | Sin acoplamiento con Hono ni con la API | V4 superada; E6 en Cumple |
| AC-19 | Decisión reproducible | Informe con criterios congelados, versiones fijadas, evidencia etiquetada, resultados y decisión del propietario registrada |

Controles metodológicos: **CM1** evidencia etiquetada · **CM2** separación de fases · **CM3** sin datos reales ni código de producción · **CM4** criterios congelados.

## 13. Candidatos y versiones evaluadas

| Candidato | Versión evaluada (`latest`, 2026-09-27) | RESEARCH 1 | RESEARCH 2 |
|---|---|---|---|
| Next.js | `next` 16.3.6 | Sí | Sí |
| React Router (modo framework) | `react-router` 8.4.0 | Sí | Sí |
| Astro | `astro` 7.3.5 | Sí | Sí |
| SvelteKit | `@sveltejs/kit` 2.70.3 | Sí | Sí |
| Nuxt | `nuxt` 4.5.2 | Sí | Sí |
| Angular con SSR | `@angular/*` 22.2.0 | Sí | **No** (sección 18) |
| SolidStart | `@solidjs/start` 2.0.5 | Sí | Sí |
| Referencia de control | Servidor Node sin framework de interfaz, en Node 24 LTS | Sí (fuera del filtro) | No |

Hono queda excluido por DS-DEC-033 (D6).

## 14. RESEARCH 1 (2026-09-27, 13:59–14:05)

### 14.1 Ejecución y accesibilidad

- 78 peticiones M-P y 2 peticiones de diagnóstico fuera de M-P para registrar el motivo de dos bloqueos: `api.github.com` sin `--fail` y `nextjs.org` mediante `curl -p`. Ambas devolvieron 403 y no se reintentaron.
- Los 11 repositorios de terceros se añadieron a la sesión en modo lectura. La respuesta fue *"read_available — Nothing was attached"*, es decir, solo lectura git anónima. No se clonó ninguno.
- **Accesibles:** `registry.npmjs.org`, `raw.githubusercontent.com` y `nodejs.org`.
- **Bloqueados:**
  - por la **política de red del entorno** (CONNECT 403 del proxy de salida): `nextjs.org`, `reactrouter.com`, `docs.astro.build`, `svelte.dev`, `nuxt.com`, `angular.dev`, `docs.solidjs.com` y `www.w3.org`;
  - por el **control de acceso GitHub de la sesión** (403, *"GitHub access to this repository is not enabled for this session"*): `github.com` y `api.github.com`.

### 14.2 Resultados E1–E8

Estado tras la corrección de auditoría aplicada a esta ejecución. Toda la evidencia es `P`; el detalle está en el anexo A.

| Candidato | E1 | E2 | E3 | E4 | E5 | E6 | E7 (a/b/c/d) | E8 |
|---|---|---|---|---|---|---|---|---|
| Next.js 16.3.6 | U | U | U | U | C | U | U (U/C/U/U) | U |
| React Router 8.4.0 | C | U | U | C | C | U | U (C/C/U/C) | U |
| Astro 7.3.5 | U | U | U | C | C | U | U (U/C/U/C) | U |
| SvelteKit 2.70.3 | C | U | U | C | C | U | U (U/C/U/U) | U |
| Nuxt 4.5.2 | U | U | C | U | C | U | U (C/C/U/C) | U |
| Angular 22.2.0 | NC (provisional) | U | U | C | C | U | U (C/C/U/C) | U |
| SolidStart 2.0.5 | U | U | U | U | U | U | U (U/C/U/C) | U |

**Es un resultado histórico de la RESEARCH 1.** E4 y E5 de SolidStart pasaron a C en la RESEARCH 2 (sección 15). El estado vigente es el de la sección 16.

Evidencia por criterio:

- **E1:**
  - React Router: `@react-router/dev` y `@react-router/node` 8.4.0 declaran como dependencia par `typescript: "^5.1.0 || ^6.0.0 || ^7.0.0"`.
  - SvelteKit: `@sveltejs/kit` 2.70.3 declara `typescript: "^5.3.3 || ^6.0.0"` (opcional).
  - Angular: `@angular/compiler-cli` y `@angular/build` 22.2.0 declaran `typescript: ">=6.0 <6.1"` (sección 18).
  - Next.js, Astro, Nuxt y SolidStart: sin rango de TypeScript en los metadatos oficiales y con la documentación bloqueada. El `typescript: "*"` de `vue` no es una declaración de Nuxt.
- **E2:** `engines.node` incluye Node 24 LTS en todos:
  - next `>=20.9.0`, react-router `>=22.22.0`, astro `>=22.12.0`, @sveltejs/kit `>=18.13`;
  - nuxt `^22.19.0 || ^24.11.0 || >=26.0.0`, @angular/* `^22.22.3 || ^24.15.0 || >=26.0.0`, @solidjs/start `>=24`.

  La LTS vigente es la v24.21.0 (nodejs.org). **`engines.node` no demuestra la ejecución como servidor autónomo en contenedor**, así que E2 queda en U en todos.
- **E3 (Nuxt):** su `SECURITY.md` considera fuera de alcance *"putting secrets in `runtimeConfig.public`, or otherwise rendering private runtime config to the client"*, lo que documenta la separación entre configuración pública y privada. El resto, U por documentación bloqueada.
- **E4** (evidencia `P` mínima: descripciones oficiales de los paquetes):
  - `@react-router/serve`: *"Production application server for React Router"*; `@react-router/node`: *"Node.js platform abstractions for React Router"*;
  - `@astrojs/node`: *"Deploy your site to a Node.js server"*;
  - `@sveltejs/adapter-node`: *"Adapter for SvelteKit apps that generates a standalone Node server"*;
  - `@angular/platform-server`: *"Angular - library for using Angular in Node.js"*; `@angular/ssr`: *"Angular server side rendering utilities"*.

  Next.js, Nuxt y SolidStart: sin evidencia accesible en esta ejecución.
- **E5:** sección 17. En esta ejecución `@solidjs/start@2.0.5` quedó en U: el campo `license` es `null` y el LICENSE MIT de la raíz del repositorio no queda vinculado al paquete.
- **E6 y E8:** U en todos, por documentación bloqueada.
- **E7:** sección 21.

### 14.3 Referencia de control (fuera del filtro y del máximo de 3)

- **Node 24:** inicio 2025-05-06, LTS desde 2025-10-28, mantenimiento desde 2026-10-20, fin de vida 2028-04-30. Node 26 LTS desde 2026-10-28 (`nodejs/Release`, `schedule.json`). Última LTS: v24.21.0 (nodejs.org).
- **`node:http`:**
  - enrutado con `message.url`;
  - cualquier cabecera de respuesta con `response.setHeader` y `writeHead` (incluidas `Cache-Control` y `ETag`) y estado 304 con `statusCode`;
  - lectura de las cabeceras de la petición (incluida `If-None-Match` y una cabecera configurable para V6) con `message.headers`;
  - respuestas en streaming con `response.write` y evento `upgrade`.

  Fuente: documentación v26.10.0; esas API existen desde la v0.x.
- **Canal de seguridad:** `SECURITY.md` de `nodejs/node` (HackerOne, política de divulgación, avisos por `nodejs-sec` y en el blog de vulnerabilidades).

### 14.4 C1–C11

No se ejecutaron: no se asignó ningún estado comparativo (sección 23).

## 15. RESEARCH 2 (2026-09-27, 15:22–15:24)

- **Alcance autorizado:** E1–E8 de seis candidatos (Angular excluido), sin repetir evidencia ya en C o NC, e incluyendo E4 de Next.js, Nuxt y SolidStart y E5 de `@solidjs/start@2.0.5`.
- **12 peticiones nuevas.** El material de la RESEARCH 1 se reutilizó sin volver a descargarlo.
- **Comprobación única de los hosts bloqueados (15:22):** los 6 hosts de documentación siguen con CONNECT 403. `api.github.com` y `github.com` siguen con 403 del control de acceso GitHub. No se reintentó.
- **E5 de `@solidjs/start@2.0.5`: U → C.**
  - Se descargó sin instalar el tarball publicado `https://registry.npmjs.org/@solidjs/start/-/start-2.0.5.tgz` (104 198 bytes).
  - Integridad verificada contra el registro: el sha512 coincide con `dist.integrity` (`sha512-Kji79Kan867rKAx8iG0W4BXzj/J2nXyGwgS6+mNA4vqLabqLMeXfY/O910JnMLHRKMZZ9Xfr4hh+DYUDuSSPCw==`) y el sha1 con `dist.shasum` (`97fc34f672f31e192a84d1b5919331799a69c23d`).
  - `package/LICENSE` contiene el texto MIT completo, leído. Tiene 1070 bytes y SHA-256 `be995fc1282c92b904501dbf46566d71008c59f899588024add3405b02cc0d00`, idéntico al LICENSE de la raíz del repositorio.
  - `package/package.json` sigue sin campo `license`. No es una contradicción: el LICENSE del propio paquete publicado vincula MIT al paquete y la versión.
- **E4 de SolidStart: U → C.**
  - El README del paquete publicado 2.0.5 es idéntico byte a byte entre el documento del registro ya descargado y `package/README.md` del tarball verificado (4012 bytes, SHA-256 `845f00eaab95212f94deba1d59b1374d5fa0d94880896c218844ca0c095021d3`).
  - Dice: *"Presets also include runtimes like Node.js, Bun, or Deno. For example, a preset like `node-server` enables hosting on your server."*
  - Es evidencia `P` mínima, con el mismo estándar que E4 de React Router, Astro y SvelteKit.
- **Lecturas sin cambio de estado:**
  - documentos del registro de `@sveltejs/adapter-node` y `@react-router/serve`: README vacío;
  - documento del registro de `@astrojs/node`: README de 1270 bytes, leído completo; remite a la documentación bloqueada;
  - README de `astro` en el documento del registro de la RESEARCH 1: leído, sin evidencia para E1–E8.
- Ningún No cumple nuevo.

## 16. Tabla consolidada E1–E8 (seis candidatos, tras la RESEARCH 2)

| Candidato | E1 | E2 | E3 | E4 | E5 | E6 | E7 | E8 |
|---|---|---|---|---|---|---|---|---|
| Next.js 16.3.6 | U | U | U | U | C | U | U | U |
| React Router 8.4.0 | C | U | U | C | C | U | U | U |
| Astro 7.3.5 | U | U | U | C | C | U | U | U |
| SvelteKit 2.70.3 | C | U | U | C | C | U | U | U |
| Nuxt 4.5.2 | U | U | C | U | C | U | U | U |
| SolidStart 2.0.5 | U | U | U | C | C | U | U | U |

Criterios que siguen en U y motivo:
- **M1:** documentación bloqueada por la política de red.
- **M2:** GitHub bloqueado por el control de acceso de la sesión.
- **M3:** sin `SECURITY.md` en las rutas consultadas (404); las alternativas no están autorizadas.
- **M4:** sin política de versiones en las fuentes accesibles.
- **M5:** sin rango de TypeScript en los metadatos oficiales.
- **M6:** evidencia accesible insuficiente.

| Candidato | Criterios en U (motivo) |
|---|---|
| Next.js | E1 (M5, M1) · E2 (M1) · E3 (M1) · E4 (M1) · E6 (M1) · E7(a) (M1) · E7(c) (M2) · E7(d) (M3, M1) · E8 (M1) |
| React Router | E2 (M1) · E3 (M1) · E6 (M1) · E7(c) (M2) · E8 (M1) |
| Astro | E1 (M5, M1) · E2 (M1) · E3 (M1) · E6 (M1) · E7(a) (M4, M1) · E7(c) (M2) · E8 (M1) |
| SvelteKit | E2 (M1) · E3 (M1) · E6 (M1) · E7(a) (M4, M1) · E7(c) (M2) · E7(d) (M3, M1) · E8 (M1) |
| Nuxt | E1 (M5, M1) · E2 (M1) · E4 (M1) · E6 (M1) · E7(c) (M2) · E8 (M1) |
| SolidStart | E1 (M6: *"TypeScript: Full integration"* sin versión; M1) · E2 (M6: `node-server` sin contenedor; M1) · E3 (M1) · E6 (M1) · E7(a) (M4, M1) · E7(c) (M2) · E8 (M6: *"API Routes"* sin cabeceras por ruta; M1) |

## 17. E5 por paquete

Metadatos oficiales del registro (`P`, JSON leído completo), 2026-09-27.

| Candidato | Paquete | Versión | Licencia | Estado | E5 agregado |
|---|---|---|---|---|---|
| Next.js | next · react · react-dom | 16.3.6 · 19.3.0 · 19.3.0 | MIT (todos) | C | **C** |
| React Router | react-router · @react-router/node · @react-router/serve · @react-router/dev · react · react-dom | 8.4.0 (×4) · 19.3.0 (×2) | MIT (todos) | C | **C** |
| Astro | astro · @astrojs/node | 7.3.5 · 11.1.6 | MIT (todos) | C | **C** |
| SvelteKit | @sveltejs/kit · @sveltejs/adapter-node · svelte · @sveltejs/vite-plugin-svelte | 2.70.3 · 5.5.7 · 5.57.1 · 7.3.1 | MIT (todos) | C | **C** |
| Nuxt | nuxt · vue | 4.5.2 · 3.5.43 | MIT (todos) | C | **C** |
| Angular | @angular/core, common, compiler, compiler-cli, platform-browser, platform-server, router, ssr, build | 22.2.0 (todos) | MIT (todos) | C | **C** |
| SolidStart | solid-js · @solidjs/router · @solidjs/start | 1.9.15 · 1.0.0 · 2.0.5 | MIT · MIT · MIT (`@solidjs/start`: según el `LICENSE` del paquete publicado, sección 15; campo `license` ausente) | C | **C** |

La lista de paquetes del perímetro salió de los metadatos, porque la documentación oficial de instalación estaba bloqueada.

## 18. Angular 22.2.0

- Versión evaluada: `@angular/*` 22.2.0 (`latest` el 2026-09-27).
- **E1 = No cumple (provisional).** `@angular/compiler-cli` y `@angular/build` 22.2.0 exigen `typescript: ">=6.0 <6.1"`, que es incompatible con TypeScript 5.9.x (DS-DEC-034).
- Otros resultados de la RESEARCH 1: E4 C, E5 C, E7(a) C, E7(b) C, E7(d) C; el resto en U.
- **No se incluyó en la RESEARCH 2** y no es candidato mientras se mantenga ese resultado.
- **No se sustituye automáticamente por Angular 21.** Evaluar la rama `v21-lts` sería una decisión separada con autorización expresa. Datos conocidos, sin evaluación: `21.2.24`, TypeScript `>=5.9 <6.1`, LTS hasta 2027-06 según `releases.md`.

## 19. SolidStart 2.0.5

- **E5 = Cumple.** La evidencia `P` es el `LICENSE` MIT contenido en el tarball publicado exacto `@solidjs/start@2.0.5`, con la integridad verificada contra `dist.integrity` y `dist.shasum` del registro (sección 15). **No** se dedujo del LICENSE de la raíz del repositorio.
- **E4 = Cumple.** La evidencia `P` mínima es el README del mismo artefacto y versión: preset `node-server` para alojarlo en un servidor propio.
- E5 agregado: Cumple (solid-js, @solidjs/router y @solidjs/start, todos MIT).
- Siguen en U: E1, E2, E3, E6, E7 y E8 (sección 16).

## 20. E7(c)

- **E7(c) = `UNVERIFIED` para los seis candidatos.**
- **Evidencia necesaria:** el estado de archivado del repositorio oficial (campo `archived` de `api.github.com/repos/{owner}/{repo}`, o el aviso equivalente en `github.com`) y la ausencia de una declaración oficial de obsolescencia.
- **Repositorios oficiales**, según el campo `repository` del registro [`P`]: `vercel/next.js`, `remix-run/react-router`, `withastro/astro`, `sveltejs/kit`, `nuxt/nuxt`, `solidjs/solid-start` (directorio `packages/start`).
- **Motivo exacto:** `api.github.com` y `github.com` responden 403 con *"GitHub access to this repository is not enabled for this session"*. El túnel se establece, así que es el **control de acceso GitHub de la sesión**, no la lista de dominios de red. Comprobado en la RESEARCH 1 (14:01) y en la RESEARCH 2 (15:22).
- **No existe una vía autorizada de solo lectura** con las capacidades actuales:
  - `raw.githubusercontent.com` y `registry.npmjs.org` no exponen el estado de archivado (el campo `deprecated` es del paquete);
  - la lectura git anónima no expone `archived`, y Git no estaba autorizado en la investigación;
  - las herramientas GitHub de la sesión están limitadas al repositorio del proyecto;
  - WebFetch no produce evidencia `P`.
- La única vía a la API que indica el entorno es **`access:"push"`, que no está autorizada**.
- **La excepción del parámetro 7 no está autorizada.**
- Parte (ii), no declarado obsoleto: ninguna versión evaluada tiene la marca `deprecated` en el registro [`P`]. Eso cubre el paquete, no el repositorio, y no sustituye a la parte (i).

## 21. Estado consolidado de E7

| Candidato | (a) | (b) | (c) | (d) | E7 |
|---|---|---|---|---|---|
| Next.js | U | C | U | U | **U** |
| React Router | C | C | U | C | **U** |
| Astro | U | C | U | C | **U** |
| SvelteKit | U | C | U | U | **U** |
| Nuxt | C | C | U | C | **U** |
| SolidStart | U | C | U | C | **U** |
| Angular 22.2.0 (solo RESEARCH 1) | C | C | U | C | **U** |

- **(a):**
  - React Router: su `SECURITY.md` da soporte a 8.x y 7.x, y no a 6.x ni anteriores.
  - Nuxt: la hoja de ruta indica 4.x estable desde 2025-07-16, con fin de vida 6 meses después de 5.x; 5.x está prevista para el cuarto trimestre de 2026 (estimado); 3.x terminó el 2026-07-31; y hay un compromiso de al menos 6 meses de soporte tras la siguiente versión mayor.
  - Angular: `releases.md` indica v22 activa (publicada el 2026-06-03, activa hasta 2027-06 y LTS hasta 2028-06), v21 en LTS hasta 2027-06 y v20 en LTS hasta 2026-11-28.
- **(b)** versiones estables en la ventana (última):
  - next 83 (16.3.6, 2026-09-22) · react-router 34 (8.4.0, 2026-09-15) · astro 115 (7.3.5, 2026-09-24);
  - @sveltejs/kit 72 (2.70.3, 2026-08-18) · nuxt 30 (4.5.2, 2026-08-05) · @angular/core 100 (22.2.0, 2026-09-23) · @solidjs/start 10 (2.0.5, 2026-09-10).
- **(d):**
  - React Router: aviso por GitHub Security Advisory.
  - Astro: aviso de seguridad, respuesta en 3 días hábiles y divulgación a 90 días.
  - Nuxt: pestaña Security o `security@nuxtjs.org`.
  - Angular: Google OSS Vulnerability Reward Program.
  - SolidStart: `.github/SECURITY.md`, por correo a `security@solidjs.com`.
  - Next.js y SvelteKit: sin `SECURITY.md` en `SECURITY.md`, `.github/SECURITY.md` ni `docs/SECURITY.md` (404). Sus repositorios `.github` de organización no están autorizados.
- Por E7(c), **E7 = U en todos**.

## 22. Filtro documental

- **0 candidatos superan actualmente el filtro documental** con las reglas congeladas.
- **`UNVERIFIED` ≠ No cumple.**
- Esto **no** significa que los seis frameworks sean técnicamente inadecuados. Significa que **no se pudo completar la evidencia necesaria con las autorizaciones y el acceso actuales**. Los seis candidatos quedan fuera únicamente por criterios eliminatorios en `UNVERIFIED` derivados de restricciones de acceso.
- Aunque se resolvieran todos los demás U, el filtro seguiría en 0 mientras E7(c) siga en U.
- El único incumplimiento documentado es el de Angular 22.2.0 en E1 (provisional), y Angular no está en la RESEARCH 2.

## 23. C1–C11, PoC, framework y DS-DEC-003

- **C1–C11: no ejecutados.** Solo se aplican a quienes superen el filtro.
- **PoC: no ejecutada ni autorizada.**
- **Framework: ninguno seleccionado.** No hay recomendación de selección.
- **DS-DEC-003: sigue en `PENDING`.**

## 24. Estado actual y bloqueo

- PL-04 está **en curso y en pausa por E7(c)**.
- El bloqueo principal es de infraestructura: el control de acceso GitHub de la sesión impide verificar E7(c).
- Además, la política de red bloquea la documentación oficial de los seis candidatos (E1–E4, E6–E8).
- No hay ninguna vía autorizada de solo lectura para resolver E7(c) sin cambiar permisos.

## 25. Opciones metodológicas (no se selecciona ninguna)

1. **Habilitar la lectura de GitHub desde fuera**, sin `access:"push"` y sin aumentar permisos, para los seis repositorios oficiales. Después, verificar `archived` con M-P.
2. **Mantener PL-04 en pausa**, con el filtro en 0.
3. **Aplicar expresamente la excepción del parámetro 7** a candidatos concretos.

Elegir una opción corresponde al propietario.

## 26. Pendientes y observaciones

### 26.1 Pendientes que no son E7(c)

- Habilitar en la política de red los hosts de documentación bloqueados, para resolver E1–E4, E6–E8 y E7(a)/(d).
- Posibles autorizaciones adicionales para decisión del propietario: repositorios `vercel/.github` y `sveltejs/.github` (E7(d)); leer sin instalar los paquetes publicados de otros candidatos; dominios condicionales.
- Decisiones del PLAN aún abiertas:
  - suposiciones mínimas sobre los requisitos de la sección 4.3;
  - si DS-DEC-003 espera a revalidar con Node 26 o lo registra como condición;
  - de quién es el contrato de los endpoints de la web, que PL-04 no define.
- Contrastar con la documentación oficial la evidencia mínima de E4 y la lista de paquetes del perímetro E5, solo si el propietario lo autoriza.

### 26.2 Observaciones fuera de alcance (sin decisión)

- Angular 22 exige TypeScript 6.0, y React Router y SvelteKit ya aceptan TypeScript 6 y 7. Puede afectar en el futuro a DS-DEC-034.
- Node 24 pasa a LTS de mantenimiento el 2026-10-20.
- Nuxt 5 está prevista para el cuarto trimestre de 2026 (estimado).
- `@solidjs/start` 2.0.x publica `package.json` sin campo `license`.

## 27. Historial de la investigación y correcciones (2026-09-27)

1. **AUDIT** de PL-04 (solo lectura): lista para PLAN.
2. **PLAN:** redactado, corregido (10 correcciones del propietario) y aprobado con los parámetros 1–15.
3. **Preparación de la RESEARCH:**
   - lista de dominios;
   - GitHub Advisory Database como fuente `S`;
   - E1 reformulado; perímetro de E5 con las bibliotecas base; regla de Parcial en los eliminatorios;
   - definición literal de `P-r`; agregación de E5;
   - M-P aprobado; lectura completa de fuentes estructuradas; redirecciones por host exacto;
   - autorización de los dominios núcleo, de `raw.githubusercontent.com` y `api.github.com` limitados, y de los repositorios en solo lectura.
4. **RESEARCH 1** (13:59–14:05).
5. **Auditoría de la RESEARCH 1 y correcciones:**
   - `@solidjs/start@2.0.5` en E5, de C a U (no se deduce del LICENSE de la raíz);
   - distinción entre `UNVERIFIED` y No cumple;
   - `latest` registrado como criterio operativo;
   - Angular E1 en NC provisional;
   - E7(c) en U sin `access:"push"`;
   - fuentes raw sin fijar a commit;
   - E2 en U cuando solo existe `engines.node`.
6. **Ratificaciones:** `latest` ratificado; Angular 21 no se añade; E7(c) sigue en U.
7. **RESEARCH 2** (15:22–15:24): E5 y E4 de SolidStart pasan a C.
8. **Análisis de E7(c)** y verificación de capacidades: no existe una vía de solo lectura autorizada.
9. **Verificación previa al commit y fase DOCUMENT:** este informe.

## Anexo A. Evidencia utilizada

- Etiqueta `P` en todas las fuentes, descargadas y leídas completas según la sección 5.2. Hora en `America/Santo_Domingo`.
- El JSON del registro se leyó por análisis determinista completo. En `nodejs.org`, el texto se extrajo de forma determinista y se leyó completo.
- Las fuentes de `raw.githubusercontent.com` están tomadas **por rama** (`main`/`canary`) y **no están fijadas a un SHA de commit**, porque no pudo verificarse con `api.github.com` bloqueado. El SHA-256 es un hash del contenido, no un SHA de commit Git.
- Los artefactos brutos (cuerpos, cabeceras y registro de peticiones) se conservaron en el directorio temporal de la sesión y **no se versionan**. Este anexo registra lo necesario para volver a verificarlos.

| # | Ejecución | Hora | URL | Bytes | SHA-256 del contenido |
|---|---|---|---|---|---|
| 1 | R1 | 13:59:59 | `https://registry.npmjs.org/next/latest` | 3069 | `80fde3eccb93e4730ba9e8f80dc7076ee637101c1978737d9a7c337cf88ec569` |
| 2 | R1 | 13:59:59 | `https://registry.npmjs.org/react/latest` | 2155 | `c9b7f5adeab67cd3146ffc69712aee7ddc37c23e656e41feaaa5624dff4e02aa` |
| 3 | R1 | 13:59:59 | `https://registry.npmjs.org/react-dom/latest` | 3422 | `f7bb2eaf8d193af94c0b1749f05d7a0a8475c3d913aec87f1cb93e56ac860877` |
| 4 | R1 | 14:00:00 | `https://registry.npmjs.org/react-router/latest` | 4076 | `108564184695dfa799ea255f26825ae7922e733f7872d9ef2f92793e878e2791` |
| 5 | R1 | 14:00:00 | `https://registry.npmjs.org/@react-router%2fnode/latest` | 2345 | `8b3afbfc04ce88e847d0b2df9fe210dc893cf94dfc3d34baeeadbae1ad2b0b44` |
| 6 | R1 | 14:00:04 | `https://registry.npmjs.org/@react-router%2fserve/latest` | 2395 | `9d59a2431fed6c3163f63d943c6cd91d082112cd6e363680031c1230ee60faa9` |
| 7 | R1 | 14:00:07 | `https://registry.npmjs.org/@react-router%2fdev/latest` | 4718 | `3f2a4603603d7b07c786b6d7cf7a73e811293dfab9e22eeca0cf1988fead487b` |
| 8 | R1 | 14:00:07 | `https://registry.npmjs.org/astro/latest` | 8170 | `a479ff7c092053e397f6d2fd419bc17cfcbf4da33ad328f408a027876de53a22` |
| 9 | R1 | 14:00:07 | `https://registry.npmjs.org/@astrojs%2fnode/latest` | 2619 | `6f69d3649d8a8b32da5dfa33e2a3f49663dfe38387c676d1c094065908159ee2` |
| 10 | R1 | 14:00:08 | `https://registry.npmjs.org/@sveltejs%2fkit/latest` | 5495 | `56686ba949a9475814292cba2c32748caa6ae261d4614e3dff56e11e972bf88c` |
| 11 | R1 | 14:00:08 | `https://registry.npmjs.org/@sveltejs%2fadapter-node/latest` | 2644 | `ba12209c37205f40dba9f179afc9626cc0bf2683f5effab6ce9af742dc23b270` |
| 12 | R1 | 14:00:09 | `https://registry.npmjs.org/svelte/latest` | 6058 | `7c3145d2a8ed812f985f1958c86c5eafa5986e9488da26afa415955493435c73` |
| 13 | R1 | 14:00:10 | `https://registry.npmjs.org/@sveltejs%2fvite-plugin-svelte/latest` | 2811 | `97f1ce889d24858aea32b1789ebf89096a4c0013108515072139c0e173b51a8e` |
| 14 | R1 | 14:00:10 | `https://registry.npmjs.org/nuxt/latest` | 4731 | `ddbed22a2ea5e2b8fd3314f69cb48b985a8d08b840112af82c69f463d0d98846` |
| 15 | R1 | 14:00:11 | `https://registry.npmjs.org/vue/latest` | 3214 | `620d675853c73e1d67bed2a59762b887a314a04cdc588d8f833c4b33fa88fe04` |
| 16 | R1 | 14:00:11 | `https://registry.npmjs.org/@angular%2fcore/latest` | 3351 | `d2fc387cccf2917ead7722af1206669e817e45d16107a55be51024c34e77d4d5` |
| 17 | R1 | 14:00:12 | `https://registry.npmjs.org/@angular%2fcommon/latest` | 2822 | `fbe197330e4acb0907311051b5ad07babda46286dd064316d277022b3e02f897` |
| 18 | R1 | 14:00:12 | `https://registry.npmjs.org/@angular%2fcompiler/latest` | 2261 | `ad8ea5233954ad6912653b4d54a2e00131320e32cad109963f6a139879344291` |
| 19 | R1 | 14:00:13 | `https://registry.npmjs.org/@angular%2fplatform-browser/latest` | 2787 | `0db25bb8181f1e70d393c37b51689a6395c9e85529e7e28087c62858b093e8ec` |
| 20 | R1 | 14:00:14 | `https://registry.npmjs.org/@angular%2fplatform-server/latest` | 2678 | `dd8ce1fe365d1e36de3a28b39fc1e2d12fa5cdca4fedfaf5e8a7202ef2b7c6ab` |
| 21 | R1 | 14:00:14 | `https://registry.npmjs.org/@angular%2frouter/latest` | 2621 | `d519da6ffa8e211a73ecdaa1a265872900d4297fbe6d5a388a1efa2bec54e327` |
| 22 | R1 | 14:00:14 | `https://registry.npmjs.org/@angular%2fssr/latest` | 2442 | `ed5073739b6e8133ef58664811cf93a0731585e8b8808b2d044706eef44dc6dc` |
| 23 | R1 | 14:00:15 | `https://registry.npmjs.org/@angular%2fbuild/latest` | 3613 | `b5710f7a112a69668b96f1031182b294825e578a23ae08476441f12ceec60df8` |
| 24 | R1 | 14:00:15 | `https://registry.npmjs.org/@angular%2fcompiler-cli/latest` | 2949 | `c2c5247da87c50a31de2f44b05a4b88edf39146f9b78edaa754766925017b16b` |
| 25 | R1 | 14:00:15 | `https://registry.npmjs.org/@solidjs%2fstart/latest` | 3034 | `97c943616b1ca64417747200de14d2d760721af5edc4715dc31c35c1eaa6253b` |
| 26 | R1 | 14:00:16 | `https://registry.npmjs.org/solid-js/latest` | 7511 | `4b017e9a628071d9e1aa7c1485e4472bbb31a79c4b5e5d2a1421aa0e1b5e5240` |
| 27 | R1 | 14:00:17 | `https://registry.npmjs.org/@solidjs%2frouter/latest` | 2530 | `f975a3edbffe7c4d2e8bc085194cc974b1fe7d11971f244bad3b5245b23a82c2` |
| 28 | R1 | 14:00:38 | `https://registry.npmjs.org/next` | 31320299 | `caa3acbe084a599d703789f7f27fd031ccf9e12d0bacc0802ff985be42f64be0` |
| 29 | R1 | 14:00:38 | `https://registry.npmjs.org/react-router` | 4114593 | `59d3ead377756f27d695c6971c8b4984e9aba8d4e39e80a846e241cb968024da` |
| 30 | R1 | 14:00:38 | `https://registry.npmjs.org/astro` | 9521149 | `47005d9f19a8a744c5cdf9440b52a4662e0aeaf395fd5b2d72c7ef77b17b0b90` |
| 31 | R1 | 14:00:39 | `https://registry.npmjs.org/@sveltejs%2fkit` | 4239324 | `4d6291b7b3535d6a3230ecd923fac3f5f7a9d4e6e1a55d2f11086426affee4c1` |
| 32 | R1 | 14:00:39 | `https://registry.npmjs.org/nuxt` | 1602905 | `28247dc58160774808995b2bfb4515905568f559632d408323b0fb4986a493a8` |
| 33 | R1 | 14:00:39 | `https://registry.npmjs.org/@angular%2fcore` | 3194893 | `5a86bbdfeb96fb77175560f3959faaf8e6fec70ca860c58f6a2fe0a7d302d957` |
| 34 | R1 | 14:00:39 | `https://registry.npmjs.org/@angular%2fcompiler-cli` | 3459735 | `6eafd52ba564b62b32c07f35bda1b3cf1cc7ff1ccd22c2b28475b3f8c23f0921` |
| 35 | R1 | 14:00:39 | `https://registry.npmjs.org/@solidjs%2fstart` | 280160 | `fac5a7dbd2405d93665ce6de3dd550bf5692c114412407c1a7a0d69e82b75ca6` |
| 36 | R1 | 14:01:34 | `https://raw.githubusercontent.com/nodejs/node/main/SECURITY.md` (rama, sin fijar a commit) | 37842 | `f0ba3224dfc4d72a2625a6d1344c7c4969824c8fc3a36f7ac880382bc581f6f2` |
| 37 | R1 | 14:01:57 | `https://raw.githubusercontent.com/remix-run/react-router/main/SECURITY.md` (rama, sin fijar a commit) | 2064 | `de544bc7363609b42716b329c7eb1580fbdd74b59fa85d43fbf50a0e03b21fd5` |
| 38 | R1 | 14:01:57 | `https://raw.githubusercontent.com/withastro/astro/main/SECURITY.md` (rama, sin fijar a commit) | 2116 | `19c14e3af1f64d65a8143e8d91b7ccc3948987b0136955adb50da8c1b308657b` |
| 39 | R1 | 14:01:59 | `https://raw.githubusercontent.com/nuxt/nuxt/main/SECURITY.md` (rama, sin fijar a commit) | 7791 | `78e78bc9afaebe9dd296f4e5c8aba58720e1990c48620f140f4eda2aa048ea58` |
| 40 | R1 | 14:01:59 | `https://raw.githubusercontent.com/angular/angular/main/SECURITY.md` (rama, sin fijar a commit) | 413 | `aecd1819f7b726af10f222ad46410e03616372c12d5c57c89d2dcb1b8cfa6523` |
| 41 | R1 | 14:01:59 | `https://raw.githubusercontent.com/angular/angular-cli/main/SECURITY.md` (rama, sin fijar a commit) | 413 | `aecd1819f7b726af10f222ad46410e03616372c12d5c57c89d2dcb1b8cfa6523` |
| 42 | R1 | 14:02:00 | `https://raw.githubusercontent.com/solidjs/solid-start/main/.github/SECURITY.md` (rama, sin fijar a commit) | 1862 | `95b9820705b76e0e9642a60730c4b3bc4ec7cf7e58ec6d2a2f9243e1d5ed70d9` |
| 43 | R1 | 14:03:27 | `https://nodejs.org/en/about/previous-releases` | 296672 | `d310a2bbdc2f961b2283d1f73618dc09208e5694cd57be7ef8005cd006b06238` |
| 44 | R1 | 14:04:51 | `https://raw.githubusercontent.com/solidjs/solid-start/main/LICENSE` (rama, sin fijar a commit) | 1070 | `be995fc1282c92b904501dbf46566d71008c59f899588024add3405b02cc0d00` |
| 45 | R1 | 14:04:51 | `https://raw.githubusercontent.com/nodejs/Release/main/schedule.json` (rama, sin fijar a commit) | 3255 | `1cf0432ceb9dfde7f1fd4cce43206519942cfdfad5a26039c2f4bb20fde8549c` |
| 46 | R1 | 14:04:51 | `https://raw.githubusercontent.com/angular/angular/main/adev/src/content/reference/releases.md` (rama, sin fijar a commit) | 15643 | `ea892335537f7ce725fa4e98ef6c64cffff56a22c8430c227f60258b8fefac9c` |
| 47 | R1 | 14:05:20 | `https://raw.githubusercontent.com/nuxt/nuxt/main/docs/5.community/6.roadmap.md` (rama, sin fijar a commit) | 8503 | `e1004f0847aad349872e37433b32a68a164eb745b21b2b3d24789c43e37c8179` |
| 48 | R1 | 14:05:34 | `https://nodejs.org/api/http.html` | 647993 | `c9e6cd07195f33067df8941acfa332554a93e49e9aecfe3823006186f838677d` |
| 49 | R2 | 15:22:18 | `https://registry.npmjs.org/@solidjs/start/-/start-2.0.5.tgz` (integridad verificada contra `dist.integrity`) | 104198 | `3c0cf5d4ce0829cab92b8bfd2ffa9413016a339091ed770a423bd19689f379e0` |
| 49a | R2 | — | `package/LICENSE` dentro del tarball n.º 49 | 1070 | `be995fc1282c92b904501dbf46566d71008c59f899588024add3405b02cc0d00` |
| 49b | R2 | — | `package/README.md` dentro del tarball n.º 49 | 4012 | `845f00eaab95212f94deba1d59b1374d5fa0d94880896c218844ca0c095021d3` |
| 50 | R2 | 15:24:12 | `https://registry.npmjs.org/@sveltejs%2fadapter-node` (sin cambio de estado) | 593372 | `d580373f804bef657e8358609502d01e4cfd09d6e88d262dc961bb37f281d456` |
| 51 | R2 | 15:24:12 | `https://registry.npmjs.org/@astrojs%2fnode` (sin cambio de estado) | 514132 | `25aa3b87d92169ad48056ae0967e8c0758447fc7310b3932da628692518ca790` |
| 52 | R2 | 15:24:12 | `https://registry.npmjs.org/@react-router%2fserve` (sin cambio de estado) | 1844276 | `882f07c17d3e6ffd4313430ada52e27a7f1e0352033e4dad0b86fd005b3a6f0b` |

## Anexo B. Peticiones sin evidencia (bloqueos y 404)

| Ejecución | Hora | URL | Resultado |
|---|---|---|---|
| R1 | 14:01:03–14:01:06 | `https://api.github.com/repos/{vercel/next.js, remix-run/react-router, withastro/astro, sveltejs/kit, nuxt/nuxt, angular/angular, angular/angular-cli, solidjs/solid-start, nodejs/node}` | 403, control de acceso GitHub de la sesión |
| R1 | 14:01:34–14:01:35 | `https://github.com/nodejs/node/security/policy`, `https://github.com/nodejs/node` | 403, control de acceso GitHub de la sesión |
| R1 | 14:01:56–14:02:00 | `https://raw.githubusercontent.com/vercel/next.js/canary/{SECURITY.md, .github/SECURITY.md, docs/SECURITY.md}`, `https://raw.githubusercontent.com/sveltejs/kit/main/{SECURITY.md, .github/SECURITY.md, docs/SECURITY.md}`, `https://raw.githubusercontent.com/solidjs/solid-start/main/SECURITY.md` | 404 |
| R1 | 14:02:49–14:02:51 | `https://{nextjs.org, reactrouter.com, svelte.dev, nuxt.com, angular.dev, docs.solidjs.com}/sitemap.xml`, `https://docs.astro.build/sitemap-index.xml` | CONNECT 403, política de red |
| R1 | 14:03:27 | `https://www.w3.org/TR/WCAG22/` | CONNECT 403, política de red |
| R1 | 14:04:50–14:05:20 | `https://raw.githubusercontent.com/solidjs/solid-start/main/packages/start/LICENSE`, `https://raw.githubusercontent.com/nuxt/nuxt/main/docs/5.community/{5,4,7}.roadmap.md` | 404 |
| R2 | 15:22:38–15:22:40 | `https://{nextjs.org, reactrouter.com, svelte.dev, nuxt.com, docs.solidjs.com}/sitemap.xml`, `https://docs.astro.build/sitemap-index.xml` | CONNECT 403, política de red |
| R2 | 15:22:40–15:22:41 | `https://api.github.com/repos/vercel/next.js`, `https://github.com/vercel/next.js` | 403, control de acceso GitHub de la sesión |
