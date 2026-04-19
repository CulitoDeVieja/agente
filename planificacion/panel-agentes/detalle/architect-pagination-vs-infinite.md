# Decisión: pagination-vs-infinite

**Decisión:** Ninguno. Completadas limit 10 (ya definido). Si user quiere ver más, botón "ver todas" en v0.2.

## Alternativas
- Paginación — complejidad sin necesidad.
- Infinite scroll — UX pobre en desktop.
- Mostrar todo — performance si lista >200.

## Justificación
Limit 10 cubre el caso "qué terminé recién". Ver todas = feature v0.2.
