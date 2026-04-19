# Tailwind CSS v4 — breaking changes

## TL;DR
- Config JS (`tailwind.config.js`) ya no se auto-detecta: reemplazado por `@theme` en CSS.
- `@tailwind base/components/utilities` → `@import "tailwindcss";` (una sola línea).
- Plugin Vite recomendado: `@tailwindcss/vite`. Browsers requeridos: Safari 16.4+, Chrome 111+, Firefox 128+.

## Detalles

**Config:** CSS-first con `@theme`. Si hay JS config legacy: `@config "../tailwind.config.js"` explícito. Sin soporte para `corePlugins`, `safelist`, `separator`.

**Imports:**
```css
@import "tailwindcss";
```

**Plugins nuevos:**
- `@tailwindcss/vite` (recomendado, más performante)
- `@tailwindcss/postcss` (dedicado, ya no viene en paquete base)
- `@tailwindcss/cli` (standalone CLI)

**CSS variables nativas:** `theme(colors.red.500)` → `var(--color-red-500)`. Todos los valores del theme se exponen como CSS vars automáticamente.

**Utilities renombradas:**
- `shadow` → `shadow-sm`, `shadow-sm` → `shadow-xs` (scale shift)
- `rounded-sm` → `rounded-xs`
- `outline-none` → `outline-hidden` (fix a11y)
- `ring` default: 3px → 1px
- `bg-opacity-*` eliminado → `bg-black/50`
- `flex-shrink-*` → `shrink-*`; `flex-grow-*` → `grow-*`

**Sintaxis:**
- `!important`: `!flex` → `flex!` (posición después)
- Variants orden: `first:*:pt-0` → `*:first:pt-0` (left-to-right)
- Custom utilities: `@layer utilities { .x {} }` → `@utility x {}`

**Defaults cambiados:** border color `gray-200` → `currentColor`; placeholder `gray-400` → `currentColor/50`; button cursor `pointer` → `default`.

**Performance:** `space-*` / `divide-*` ahora usan `:not(:last-child)` en vez de `:not([hidden])`.

**Tool de upgrade:** `npx @tailwindcss/upgrade`.

## Fuente
https://tailwindcss.com/docs/upgrade-guide

## Aplicabilidad a panel-agentes
**Sí — crítico.** Panel-agentes usa Tailwind y ya seteó la skill con v4 (`react-tailwind-setup.md`, `tailwind-dark-mode.md`). Chequear que el código del builder use:
1. `@import "tailwindcss";` (no las 3 directivas viejas).
2. Sin `bg-opacity-*` ni `flex-shrink-*`.
3. Si hay `shadow`/`rounded-sm` explícitos → ajustar al nuevo scale.
4. `outline-none` → `outline-hidden` para accesibilidad.
El dark mode setup ya usa la nueva sintaxis `@variant dark`.
