# Decisión: date-time-formatting

**Decisión:** `date-fns` con locale `es`. Formato: fecha absoluta YYYY-MM-DD; relativa "hace Xm" en lists; tooltip con ISO full.

## Alternativas
- Intl API nativo — ok pero sintaxis verbosa.
- Moment.js — deprecated, 300KB.
- Luxon — 70KB, overkill.

## Justificación
date-fns tree-shakes a ~5KB con 2 funciones (`format`, `formatDistance`).
