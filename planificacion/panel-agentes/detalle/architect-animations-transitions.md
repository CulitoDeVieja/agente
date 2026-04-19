# Decisión: Animaciones y transiciones

**Decisión:** Transiciones CSS cortas (150ms) en hover/focus de cards y botones. Fade-in 200ms al cargar data. Respetar `prefers-reduced-motion`.

## Alternativas
- **Framer Motion** — rechazado: 30KB+ para animaciones que `transition` de Tailwind cubre.
- **Cero animaciones** — rechazado: hover feedback mejora sensación de responsividad.
- **Animaciones largas (500ms+) con easing fancy** — rechazado: dashboard pide feedback inmediato, no "delightful".

## Justificación
`transition-colors duration-150` y `transition-opacity duration-200` de Tailwind alcanzan. `@media (prefers-reduced-motion: reduce)` corta todo.
