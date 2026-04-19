# Decisión: ux-animaciones-feedback

**Decisión:** Fade-in 200ms en load, toasts slide-in 150ms desde top-right, sin loading skeletons complejos.

## Alternativas
- Spinner bloqueante — tapa data previa.
- Animaciones largas con easing — distrae.
- Cero animaciones — pierde feedback de "algo pasó".

## Justificación
Movimiento breve para confirmar acción, sin robar atención. Coherente con decisión "animations-transitions".
