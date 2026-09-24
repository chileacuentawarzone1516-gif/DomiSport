# DS-SRC — Registro oficial de fuentes de datos

Registro separado de DS-DEC (DS-DEC-021).
Vocabulario: `CANDIDATE` · `UNDER_REVIEW` · `APPROVED` · `DO_NOT_USE` · `REJECTED`.

**Ninguna fuente está `APPROVED`. Ningún proveedor de datos está seleccionado.**

Principio: acceso técnico ≠ derecho legal; uso comercial ≠ redistribución; mostrar en la web ≠ servir mediante la API propia.

| ID | Fuente | Estado | Notas | Pendiente |
|---|---|---|---|---|
| DS-SRC-001 | MLB Stats API | **DO_NOT_USE** | Prohibida en producción, desarrollo, fixtures, tests, ejemplos, documentación técnica, fallback, LIDOM y adaptadores. El wrapper `python-mlb-statsapi` es MIT, pero esa licencia cubre el código, no los datos. Evidencia de la investigación original del propietario: pendiente de adjuntar | Solo se reabre con licencia o permiso escrito verificable de MLB más decisión explícita del propietario (DS-DEC-014) |
| DS-SRC-002 | Sportradar | CANDIDATE | Distribuidor exclusivo de datos oficiales de MLB hasta 2032 [P-r]. La prueba prohíbe publicar [P-r]. Redistribución y sublicencia prohibidas sin acuerdo escrito [P-r]. Cobertura actual de la LIDOM UNVERIFIED (solo rastros de 2019-20) | Precio, cobertura LIDOM, almacenamiento, histórico, redistribución, API propia, agentes, IA, latencia |
| DS-SRC-003 | SportsDataIO | CANDIDATE | Licencia no sublicenciable; no se republica sin consentimiento escrito [P-r]. La prueba usa datos alterados [P-r] | Publicación, redistribución, almacenamiento, origen, IA, coste |
| DS-SRC-004 | API-Sports | UNDER_REVIEW | Declara que no concede licencia de publicación [P-r]. El acceso técnico no equivale a licencia | Titularidad de los derechos de publicación |
| DS-SRC-005 | BALLDONTLIE | CANDIDATE | **Requiere confirmación escrita:** sus términos son contradictorios [P-r]. Cubre MLB y no cubre la LIDOM [P, especificación OpenAPI oficial]. Sin garantía de origen de los datos [P-r] | Almacenamiento, caché, API propia, redistribución, IA, agentes, origen, no competencia |
| DS-SRC-006 | MySportsFeeds | CANDIDATE | Gratis solo para uso no comercial; comercial de pago [P-r] | Precio, redistribución, almacenamiento, origen, IA, API propia |
| DS-SRC-007 | football-data.org | CANDIDATE | Solo el escenario de fútbol de pago; el plan gratuito es no comercial [P-r]. Exige atribución; hay obligaciones tras cancelar [P-r] | Términos de los planes de pago |
| DS-SRC-008 | Sportmonks | CANDIDATE | Principalmente fútbol | Términos comerciales |
| DS-SRC-009 | TheSportsDB | UNDER_REVIEW | No se asumen derechos de imágenes | Derechos de datos e imágenes |
| DS-SRC-010 | Jolpica F1 | REJECTED | Datos bajo CC BY-NC-SA 4.0: sin uso comercial sin acuerdo [P] | Solo con acuerdo comercial |
| DS-SRC-011 | OpenF1 | REJECTED | Orientado a uso no comercial; proyecto no oficial [P-r] | Solo con licencia adecuada |
| DS-SRC-012 | NBA.com stats | REJECTED | Uso comercial prohibido [P-r] | — |
| DS-SRC-013 | NHL API (no documentada) | REJECTED | Sin licencia ni documentación oficial [S] | — |
| DS-SRC-014 | ESPN (endpoints ocultos) | REJECTED | Sin derechos comerciales [S] | — |
| DS-SRC-015 | LIDOM oficial | UNDER_REVIEW | **Prioridad.** DigiSport ABH, S.A. aparece vinculada al portal, la app oficial, la anotación, las estadísticas y Digimetrics [P-r/S]. **No está confirmado** que sea titular, propietaria de los datos, licenciataria ni distribuidora autorizada | Titularidad, papel de DigiSport, API o feed autorizado, licencias, histórico, redistribución, API propia, agentes, IA, relación con MLB (cuestionario E-01) |
| DS-SRC-016 | Goalserve | CANDIDATE | Baja prioridad; cobertura de la LIDOM no confirmada | Cobertura, cadena de derechos |
| DS-SRC-017 | Genius Sports | CANDIDATE | Relevante principalmente para la NFL | Solo si la NFL entra en el alcance |
