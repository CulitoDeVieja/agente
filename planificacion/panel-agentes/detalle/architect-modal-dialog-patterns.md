# Decisión: modal-dialog-patterns

**Decisión:** 1 modal en v0.1 (config). Full-screen overlay oscuro + card centrado + close con Esc o backdrop click. Sin animación compleja.

## Alternativas
- Dialog biblioteca (Radix Dialog) — innecesario para 1 modal.
- Slide-over lateral — menos clásico para settings.
- Popover inline — ocluye contexto importante.

## Justificación
Overlay simple + focus trap manual (10 líneas). Accesibilidad básica con `role="dialog"` + `aria-modal="true"`.
