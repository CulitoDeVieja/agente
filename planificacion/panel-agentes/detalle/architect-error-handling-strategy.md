# Decisión: Estrategia de error handling

**Decisión:** 3 capas — (1) Rust retorna `Result<T, String>`, (2) JS `api.*` throws, (3) componente top-level `<ErrorBoundary>` + toasts para errores de comandos.

## Alternativas
- **Errores silenciosos + logs** — rechazado: el usuario no sabría que algo falló.
- **Modal bloqueante en cada error** — rechazado: interrumpe flujo por errores recuperables (ej: git pull offline).
- **Sentry / telemetría remota** — rechazado v0.1: app local sin cuenta de servicio; agregar solo si llega feedback de bugs silenciosos.

## Justificación
Toast no-bloqueante para errores de comandos (refresh falló → seguir con data previa). `<ErrorBoundary>` captura render errors → pantalla "algo falló, ↻ para reintentar". Config inválida → modal bloqueante hasta corregir (app no puede operar).
