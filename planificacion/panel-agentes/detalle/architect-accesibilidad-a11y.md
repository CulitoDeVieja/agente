# Decisión: Accesibilidad (a11y)

**Decisión:** Mínimo viable v0.1: HTML semántico (`<button>`, `<nav>`, `<main>`, `<h1>-<h3>`), contraste AA en paleta oscura, `aria-label` en iconos sin texto, focus visible (Tailwind `focus-visible:ring`).

## Alternativas
- **WCAG AAA completo + testing con axe** — rechazado v0.1: app mono-usuario (Antonio). No hay promesa pública.
- **Sin a11y hasta feedback** — rechazado: los defaults semánticos cuestan cero, retrofit cuesta caro.
- **Biblioteca accesible completa (Radix UI)** — rechazado: dep pesada para 4 componentes simples.

## Justificación
Defaults bien hechos (botones reales, headings jerárquicos, contrast AA) cubren 80% sin overhead. Revisar con `axe-core` solo si la app se comparte con otros usuarios.
