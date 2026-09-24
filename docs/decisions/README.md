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
