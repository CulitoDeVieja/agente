# Decisión: error-reporting-ui

**Decisión:** Toast con texto del error + botón "copiar". Sin sistema remoto (Sentry) v0.1.

## Alternativas
- Sentry/Bugsnag — user único, sin necesidad.
- Solo toast efímero — user pierde info del error al cerrarlo.
- Log file visible — overkill.

## Justificación
Toast persistente hasta que user cierra + botón copiar facilita reportar si aparece bug.
