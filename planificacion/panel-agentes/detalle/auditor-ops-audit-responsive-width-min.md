# Plan auditoría/ops: audit-responsive-width-min

**Decisión:** Verificar cobertura de UX states contra specs architect.

## Pasos concretos
1. `rg 'ErrorBoundary' src/` — debe aparecer en App root al menos.
2. `rg '<Skeleton|loading' src/` — cada fetch tiene skeleton.
3. Resize ventana a 900×600 y 1280×800 — layout no se rompe.

## Criterio pasa/no-pasa
ErrorBoundary en root. Todos los fetches tienen skeleton. Layout estable en min/ideal width.
