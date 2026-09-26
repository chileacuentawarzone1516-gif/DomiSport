# PL-03 — Framework de API (DS-DEC-033)

- Estado de PL-03: **cerrada** (2026-09-26).
- **DS-DEC-033: APPROVED WITH CONDITION** (propietario, 2026-09-26).
- Decisión: **Hono 4.x + @hono/zod-openapi + @hono/node-server**, sobre **Node.js 24 LTS**.
- **Fastify** permanece como alternativa documentada.
- **Hono no está implementado en producción.** Toda la evidencia procede de PoC aisladas en el directorio temporal de la sesión, con datos sintéticos, sin proveedores ni base de datos.

## 1. Evidencia confirmada

### 1.1 Comparación inicial de candidatos (2026-09-24)

| Criterio | Hono | Fastify | NestJS | Express 5 |
|---|---|---|---|---|
| Generador OpenAPI | zod-to-openapi (el de PL-01) [P] | `toJSONSchema` nativo de Zod + @fastify/swagger [P] | @nestjs/swagger + nestjs-zod [P] | No tiene |
| Equivalencia con PL-01 (oasdiff) | Sin cambios [P, ejecutado] | 23 diferencias marcadas como incompatibles (de representación) [P, ejecutado] | UNVERIFIED | — |
| Validación de respuestas en ejecución | No incluida [P] | Integrada [P, ejecutado] | Integrada [P] | Manual |
| Paquetes instalados en la PoC | 8 (15 MB) [P] | 63 (27 MB) [P] | — | — |
| Compatibilidad de versiones | Sí | Sí | nestjs-zod 5.5.0 no admite Nest 12 [P] | Sí |

### 1.2 Validación final (2026-09-26)

**Runtime:** Node **v24.21.0** (LTS "Krypton", 2026-09-07), binario oficial con SHA-256 verificado. A esta fecha [P, nodejs/Release]:
- Node 24: Active LTS; mantenimiento desde 2026-10-20; fin 2028-04-30.
- Node 26: Current; entra en LTS el 2026-10-28.
- Node 22: Maintenance LTS; fin 2027-04-30.

**Versiones validadas:** hono 4.13.9, @hono/zod-openapi 1.6.3, @hono/node-server 2.1.1, zod 4.6.5, @asteasolutions/zod-to-openapi 9.1.0, typescript 5.9.3, npm 11.19.0.

| Verificación | Resultado |
|---|---|
| Instalación en Node 24; una sola instancia de zod y zod-to-openapi | PASS |
| `tsc` estricto: 0 errores; un status no declarado sin cast no compila (control negativo) | PASS |
| Suite funcional: 17/17 | PASS |
| Validación de respuestas: la válida pasa; fallan enum inválido, campo ausente, tipo incorrecto, status no declarado y content-type no declarado | PASS |
| Registro de violaciones sin payload (DS-DEC-026) | PASS |
| `packages/contracts` sin importar Hono; `apps/api` y `apps/web` lo consumen; web → API por HTTP real | PASS |
| OpenAPI de Hono byte a byte idéntico al del generador independiente de `@ds/contracts` | PASS |
| Redocly: 0 errores (estructural y recommended); 1 aviso `info-license` | PASS |
| oasdiff PL-01 → Hono: 0 cambios incompatibles (3/3 deterministas); diferencias informativas y deliberadas: 304 ×2, 500 ×2 y 400 en `getEvent` | PASS |
| Regla de dependencias: 0 violaciones en limpio; 3/3 detectadas al introducir una violación | PASS |
| `onError` propio: respuesta RFC 9457 y ningún dato sensible en log ni en respuesta | PASS |
| `npm audit` (producción, 19 dependencias): 0 vulnerabilidades | PASS |
| Arranque (medición de este entorno, **no un benchmark**): app lista ~3 ms; primera respuesta ~185 ms desde el inicio del proceso; RSS ~97 MB; apagado limpio con SIGTERM | Informativo |

## 2. Evidencia no verificada

- **Throughput y latencia bajo carga: UNVERIFIED.** Incluye el coste del middleware de validación de respuestas.
- Node 26: no es LTS hasta el 2026-10-28.
- TypeScript 6/7 (fijado en 5.9.x por DS-DEC-034).
- Respuestas en streaming (SSE) con validación de contrato.
- Versión exacta que corrige cada aviso de seguridad publicado de Hono (lo cubre de forma indirecta el `npm audit` sin hallazgos).
- Salida OpenAPI real de NestJS y Express (no probados).

## 3. Riesgos

1. **El middleware de validación de respuestas lee el cuerpo completo**, así que no es compatible con respuestas en streaming. Pendiente de DS-DEC-030.
2. **Dependencias fantasma:** con npm workspaces, `apps/web` puede resolver `@ds/adapters`, `@ds/db` y `hono` sin declararlos. El gestor de paquetes no lo impide.
3. **Manejador de errores por defecto de Hono** [P, código fuente]: escribe el error completo en el log y responde `500 text/plain`.
4. Registrar rutas sin el envoltorio de contrato se saltaría la validación de respuestas.
5. Hono no tiene limitador de peticiones oficial.
6. **Avisos de seguridad publicados en middleware de Hono** (JWT, `serveStatic`, restricción de IP, límite de cuerpo, análisis de rutas) [P-r, GitHub Security Advisories].
7. Ecosistema oficial de plugins más pequeño que el de Fastify.

## 4. Condiciones (D1–D9)

| ID | Condición |
|---|---|
| D1 | **Versiones fijadas:** hono 4.13.x, @hono/zod-openapi 1.6.x, @hono/node-server 2.1.x, zod 4.6.x, @asteasolutions/zod-to-openapi 9.1.x, TypeScript 5.9.x |
| D2 | **Node 24 LTS** como runtime objetivo. Antes de pasar a Node 26 hay que revalidar |
| D3 | **Contratos independientes del framework:** `packages/contracts` no importa Hono; una sola instancia de zod y zod-to-openapi, verificada en CI |
| D4 | **Validación de respuestas obligatoria:** `checkResponse()` en `packages/contracts`; middleware y envoltorio `contractRoute()` en `apps/api`; todas las rutas se registran por el envoltorio; toda respuesta posible se declara (incluidos 304, 400 y 500) |
| D5 | **`onError` propio** con RFC 9457 y logs redactados |
| D6 | **Reglas de dependencias en CI:** `web` no importa adaptadores, base de datos, ingesta ni el framework de la API |
| D7 | **Límite de peticiones por capas** (borde/CDN, aplicación, distribuida); sin cabeceras `RateLimit` en borrador |
| D8 | **Revisión previa** en DS-DEC-038 antes de usar los middleware de seguridad de Hono (JWT, restricción de IP, `serveStatic`); política de actualizaciones y avisos |
| D9 | **Fastify como alternativa documentada**, a considerar solo antes del lanzamiento |

**Arquitectura de límite de peticiones propuesta para D7.** Con la topología de DS-DEC-023, el CDN no puede proteger la API porque la API es privada.

| Capa | Qué protege | Evidencia |
|---|---|---|
| Borde/CDN | Páginas y endpoints públicos de la web | Cloudflare [P]: Free 1 regla (filtro por ruta, contador por IP, 10 s); Pro 2 reglas; contadores por centro de datos, no globales |
| Aplicación | Servidor web (por IP de proxy de confianza) y API (por credencial de servicio) | `rate-limiter-flexible` (ISC): memoria, PostgreSQL, Redis y otros [P]. `hono-rate-limiter` 0.5.4: versión <1.0 y cabeceras en borrador [P] |
| Distribuida | Varias réplicas y socios futuros | `rate-limiter-flexible` sobre PostgreSQL, sin infraestructura nueva [P]. Redis solo con necesidad medida |

## 5. Impacto sobre DomiSport

- La arquitectura central y las reglas C20–C23 se mantienen intactas.
- C22 queda reforzada: garantías de contrato en compilación (status y esquemas declarados) y en ejecución (validación de respuestas).
- Los contratos son portables: el generador independiente produce el mismo OpenAPI que Hono, lo que reduce la dependencia del framework.
- **Fastify** permanece como alternativa. Para elegirlo faltaría revalidar su generador con los 6 criterios de PL-01, eliminar los componentes duplicados `*Input`, corregir los media types de error y la pérdida del `discriminator`, ejecutarlo en Node 24, y asumir que cambiar de framework después del lanzamiento altera la representación del contrato.
- **Hono no está implementado en producción.**

## 6. Decisiones posteriores requeridas

| Tema | Decisión o tarea |
|---|---|
| Streaming/SSE y validación de respuestas | DS-DEC-030 |
| Límite de peticiones | DS-DEC-031 / DS-DEC-038 |
| Middleware de seguridad, política de actualizaciones, proxy de confianza para la IP del cliente | DS-DEC-038 |
| Reglas de dependencias, herramienta de CI, gestor de paquetes, ubicación del envoltorio de rutas | Política de arquitectura / PL-05 (DS-DEC-035) |
| Consumo de contratos desde la web | PL-04 (DS-DEC-003) |
| Throughput bajo carga | Pruebas de rendimiento (DS-DEC-039 / IMPLEMENTATION) |
| Migración a Node 26 | Revalidación tras el 2026-10-28 |
| Licencia de la API (aviso `info-license`) | Decisión futura del propietario |
