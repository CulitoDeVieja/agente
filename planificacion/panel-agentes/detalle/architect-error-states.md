# Decisión: Error states

**Decisión:** 3 tipos de error state — (1) toast no-bloqueante para fallas de comando, (2) pantalla de ErrorBoundary para render errors, (3) modal bloqueante para config inválida.

## Alternativas
- **Un solo estilo de error (solo toast)** — rechazado: config inválida no se puede ignorar.
- **Errores in-place donde ocurren** — parcialmente: lista vacía por error vs lista vacía real → usar empty state distinto con mensaje "no se pudo cargar" + botón retry.
- **Logs solamente** — rechazado: user no sabe qué pasó.

## Justificación
Jerarquía: recuperable (toast) → catastrófico visual (ErrorBoundary) → bloqueante funcional (modal config). Cada uno tiene botón de acción (reintentar / recargar / configurar).
