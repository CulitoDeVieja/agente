# Decisión: tooltips-strategy

**Decisión:** Tooltips nativos via atributo `title` en v0.1. Si v0.2 necesita rich content (formato), Radix Tooltip.

## Alternativas
- Radix Tooltip desde v0.1 — 15KB por feature opcional.
- Tippy.js — más pesado.
- Sin tooltips — algunos iconos necesitan explicación.

## Justificación
`title=""` es free, funciona con keyboard (en Windows). Suficiente para v0.1.
