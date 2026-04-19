# Decisión: i18n-future

**Decisión:** v0.1 solo español (user es Antonio). Strings en constantes JS (no inline). Si v0.2 internacionaliza, migrar a `react-i18next`.

## Alternativas
- i18n desde v0.1 — sin valor, user único.
- Strings inline para siempre — bloquea i18n futuro.

## Justificación
Centralizar strings en `src/strings.ts` (1 archivo) mitiga costo de migración futura sin pagar infra i18n hoy.
