# Decisión: url-state-deep-link

**Decisión:** Hash URL refleja vista actual (`#/`, `#/agent/:role`). Copiable/compartible dentro de la app; no hay deep link desde fuera.

## Alternativas
- State solo en React (sin URL) — perdés back button.
- URL completa con query params — innecesario.

## Justificación
Ya decidido en architect-011-rutas-app. Este doc lo ratifica.
