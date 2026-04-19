# Decisión: Tema dark/light

**Decisión:** Dark mode fijo en v0.1. Paleta oscura minimal definida en `@theme` de Tailwind. Sin toggle.

## Alternativas
- **Toggle dark/light + `prefers-color-scheme`** — rechazado v0.1: `00-resumen.md` pide paleta oscura minimal; toggle agrega UI sin valor inmediato.
- **Light mode solo** — rechazado: Antonio pidió oscuro.
- **Tema custom por usuario** — rechazado: app mono-usuario.

## Justificación
Pedido explícito del owner. Agregar toggle si v0.2 trae feedback de uso en ambientes iluminados.

## Paleta base (tokens)
- `--color-bg`: `#0b0d10` (casi negro)
- `--color-surface`: `#14171c`
- `--color-surface-2`: `#1c2027`
- `--color-border`: `#2a2f38`
- `--color-text`: `#e5e7eb`
- `--color-text-dim`: `#9ca3af`
- `--color-accent`: `#22c55e` (verde activo)
- `--color-warn`: `#f59e0b`
- `--color-error`: `#ef4444`
