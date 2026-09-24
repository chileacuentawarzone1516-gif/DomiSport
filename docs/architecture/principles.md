# Arquitectura central y requisitos permanentes

Aprobada mediante DS-DEC-013. No se modifica sin una nueva decisión explícita del propietario.

## Flujo de datos

```text
EXTERNAL SOURCES
↓
PROVIDER ADAPTER
↓
SCHEMA VALIDATION
↓
SEMANTIC VALIDATION
↓
NORMALIZATION
↓
EXTERNAL ID → CANONICAL ID
↓
ANOMALY DETECTION
↓
QUARANTINE (IF NEEDED)
↓
DOMISPORT CANONICAL MODEL
↓
POSTGRESQL
↓
DOMISPORT API /v1
↓
WEB / MOBILE / FUTURE PARTNERS / AGENTS / WIDGETS
```

## Reglas absolutas

- La web nunca accede directamente a PostgreSQL.
- La web nunca consume directamente proveedores deportivos.
- La web consume la DomiSport API. La API es la frontera del sistema.
- Cada proveedor está aislado en su propio adaptador.
- El modelo canónico pertenece a DomiSport. Los IDs externos se resuelven a IDs canónicos.
- Toda información mantiene procedencia y linaje.
- Los datos externos son **entrada no confiable**.
- La detección de anomalías es determinista; no depende de un LLM.
- Los datos dudosos pueden ir a cuarentena y revisión humana.
- Los agentes no tienen acceso de escritura a producción.

## Flujo de mantenimiento (DS-DEC-020)

```text
Deterministic monitors → Alerts → Claude analyzes → Claude proposes → Branch / PR → CI / tests → HUMAN APPROVAL → Deploy
```

## Requisitos permanentes

| ID | Requisito |
|---|---|
| C16 | En V1 la cuarentena requiere revisión humana. El propietario revisa los casos ambiguos. Claude analiza y propone; nunca aprueba casos ambiguos. |
| C17 | El consumo por socios externos es una posibilidad futura. La arquitectura no debe bloquearlo, pero V1 no incluye infraestructura para socios. |
| C18 | "Agentes" significa agentes propios de DomiSport. Los agentes externos quedan fuera de V1 y condicionados a las licencias de cada fuente. |
| C19 | La API pública (contratos para consumidores) y las superficies internas (ingesta, administración, sincronización, operaciones) están separadas. Las superficies internas no son públicas. |
| C20 | La web nunca accede directamente a PostgreSQL ni a proveedores externos. Consume la DomiSport API. |
| C21 | La API no expone IDs de proveedores, nombres internos, formatos propietarios, errores propietarios, estados propietarios ni estructuras internas del proveedor. |
| C22 | Una sola fuente de verdad para contrato → validación → tipos → documentación → tests → detección de cambios incompatibles. |
| C23 | El adaptador manual es arquitectónicamente válido: Editor → Manual Adapter → Schema validation → Semantic validation → Normalization → Canonical model → DomiSport API. Su uso en producción depende de DS-DEC-027. |

## Principios legales transversales

- Acceso técnico ≠ derecho legal.
- Uso comercial ≠ redistribución.
- Mostrar datos en la web ≠ servirlos mediante la API propia.
- Agente interno ≠ servicio de IA externo procesando datos licenciados.
- Estados de derechos: `YES`, `NO`, `CONTRACT`, `UNVERIFIED`. `CONTRACT` nunca equivale a `YES`.

## Frescura

- No se usa "real-time" sin evidencia.
- Campos: `source_updated_at` (puede ser `NULL`), `fetched_at`, `ingested_at`, `generated_at`.
- Se miden por separado: frescura de la fuente, latencia de ingesta, antigüedad en caché y antigüedad en el cliente.
- Un `served_at` dentro de un cuerpo cacheado no es una métrica válida de frescura.
