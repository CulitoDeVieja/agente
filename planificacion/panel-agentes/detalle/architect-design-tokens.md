# Decisión: design-tokens

**Decisión:** Tokens CSS variables en `@theme` de Tailwind v4: colores, spacing, radius, shadow, font sizes. Exportados a JS si algún componente necesita valor computado.

## Alternativas
- Tokens en JS object + style prop — pierde cache de CSS.
- Figma Tokens plugin sync — rechazado v0.1, sin Figma.
- Sin tokens (hardcoded) — imposible tema futuro.

## Justificación
Variables CSS = cache nativo, theming por media query, acceso desde Rust si fuera necesario (no lo es).
