# Decisión: Estructura Tailwind

**Decisión:** Tailwind v4 con `@tailwindcss/vite`. Clases inline en JSX. Tema custom en `src/styles.css` via `@theme`. Utilities compuestas con `clsx`, no `@apply`.

## Alternativas
- **Tailwind v3 + `postcss.config.js`** — rechazado: v4 es más rápido (build) y elimina `tailwind.config.js`.
- **CSS Modules** — rechazado: mezclar paradigmas suma complejidad.
- **styled-components / Emotion** — rechazado: runtime CSS innecesario.
- **`@apply` para componentes** — evitado: termina replicando lo que ya hace `clsx` pero oculto.

## Justificación
v4 + Vite es plug-and-play. Clases inline + `clsx` componen variantes sin capas de indirección. `@theme` centraliza tokens (colores oscuros, spacing).
