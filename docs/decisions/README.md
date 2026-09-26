# Registro de decisiones — formato y reglas

## Identificadores

- Decisiones de arquitectura y producto: `DS-DEC-XXX`.
- Fuentes y proveedores de datos: `DS-SRC-XXX` (registro separado, DS-DEC-021).
- Las subdecisiones usan sufijo de letra (`DS-DEC-006-A`).
- **Ningún ID se reutiliza**, ni siquiera si la decisión queda `SUPERSEDED` o `REJECTED`.

## Vocabulario de estados DS-DEC

| Estado | Significado |
|---|---|
| `APPROVED` | Aprobada explícitamente por el propietario. |
| `APPROVED WITH CONDITION` | Aprobada explícitamente, sujeta a las condiciones registradas. |
| `PROPOSED` | Recomendación preparada; aún sin aprobación. |
| `PENDING` | Sin resolver; se decidirá más adelante. |
| `BLOCKED` | No puede decidirse hasta que se resuelva una dependencia. |
| `REQUIRES WRITTEN CONFIRMATION` | Depende de una respuesta escrita de un tercero. |
| `REJECTED` | Descartada. |
| `DO_NOT_USE` | Prohibida su utilización. |
| `SUPERSEDED` | Absorbida por otra decisión; se indica cuál. |

`SUPERSEDED` se usa únicamente dentro de DS-DEC.

## Vocabulario de estados DS-SRC

`CANDIDATE` · `UNDER_REVIEW` · `APPROVED` · `DO_NOT_USE` · `REJECTED`

Los vocabularios DS-DEC y DS-SRC no se mezclan.

## Etiquetas de evidencia

| Etiqueta | Significado |
|---|---|
| `P` | Fuente primaria leída completa (contrato, términos, documentación o repositorio oficial). |
| `P-r` | Fuente primaria localizada mediante buscador, no leída completa. |
| `S` | Fuente secundaria. |
| `UNVERIFIED` | No confirmado. |

Nunca se convierte `P-r` en `P`, `S` en `P`, ni una hipótesis en un hecho.

## Plantilla de decisión

```markdown
### DS-DEC-XXX — Título
- Estado:
- Fecha:
- Decisión:
- Condiciones:
- Dependencias:
- Evidencia disponible:
- Evidencia pendiente:
- Aprobación: (propietario, fecha) — vacío hasta que el propietario apruebe
```

## Reglas

1. Una recomendación de Claude nunca equivale a `APPROVED`.
2. Solo el propietario cambia un estado a `APPROVED` o `APPROVED WITH CONDITION`.
3. Una decisión aprobada solo se reabre con evidencia nueva que contradiga directamente una condición registrada, y lo decide el propietario.

## Fe de erratas

- Una fe de erratas corrige una **referencia errónea o un error tipográfico** sin cambiar la sustancia ni el estado de una decisión.
- Identificador: `E-<número de decisión>-<nn>` (por ejemplo, `E-033-01`).
- Requiere aprobación explícita del propietario.
- **Conserva literalmente el texto original.** Junto a él se añade la marca `[ver E-<...>]`, sin borrarlo ni sustituirlo.
- Un cambio de sustancia no es una fe de erratas: exige la regla 3 o una decisión nueva.

## Convención de fechas

- Adoptada por el propietario el 2026-09-25.
- La zona horaria oficial de DomiSport para fechas de decisiones, auditorías, incidencias y documentación es **`America/Santo_Domingo`** (UTC-4, AST, sin horario de verano).
- Las marcas de tiempo técnicas en UTC (por ejemplo, las de git) se convierten a esa zona al documentar un evento.

### Registros anteriores a la convención

Las fechas de la documentación creada antes de la convención se tomaron del reloj UTC del entorno. **No se reescriben**; se registra aquí su correspondencia:

| Registros | Evento | Fecha registrada (UTC) | Fecha en `America/Santo_Domingo` | Evidencia |
|---|---|---|---|---|
| `REGISTRY.md` (estado de DS-DEC-033), `PLAN.md` (cierre de PL-03), `PL-03-api-framework.md` (estado, aprobación de DS-DEC-033 y "Validación final") | Aprobación de DS-DEC-033, cierre de PL-03 y validación final de Hono | 2026-09-26 | **2026-09-25** | Commit `900f2b2`: 2026-09-26T01:24:24Z = 2026-09-25 21:24:24 -04:00 |
| Registros con 2026-09-24 | Materialización del registro, PL-01, PL-02 y aprobaciones de esa sesión | 2026-09-24 | 2026-09-24 para los commits `a211783` (02:13 -04:00) y `465350a` (02:22 -04:00) | Las aprobaciones anteriores a esos commits no tienen hora registrada; si alguna ocurrió antes de las 04:00 UTC correspondería al 2026-09-23 local (UNVERIFIED) |
