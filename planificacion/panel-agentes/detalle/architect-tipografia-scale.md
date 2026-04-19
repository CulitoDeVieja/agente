# Decisión: tipografia-scale

**Decisión:** Sistema: 12/14/16/18/24/32 px. `text-xs` captions, `text-sm` body-secondary, `text-base` body, `text-lg` subhead, `text-2xl` heading. Font stack system (ui-sans-serif).

## Alternativas
- Font custom (Inter, JetBrains Mono) — 50KB+ de fonts; system stack rinde bien en Windows.
- Escala modular (1.25x) — termina en valores raros como 15.625; prefiero enteros.
- Solo 2 tamaños — pobre jerarquía visual.

## Justificación
6 steps cubren dashboard + detail. System font = zero cost, zero FOUT.
