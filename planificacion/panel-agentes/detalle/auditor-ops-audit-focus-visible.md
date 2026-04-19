# Plan auditoría/ops: audit-focus-visible

**Decisión:** Audit manual + axe-core CLI contra app buildeada. Baseline WCAG AA, no AAA.

## Pasos concretos
1. `pnpm test:a11y` (axe-core sobre build dev).
2. Abrir app, tabular con Tab key por todos los controles.
3. Verificar contraste AA con DevTools.

## Criterio pasa/no-pasa
axe-core 0 violations nivel 'serious' o superior. Tab reaches every button.
