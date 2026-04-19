# Decisión: Responsive desktop

**Decisión:** Ventana fija mínima 900×600, ideal 1280×800. Grid responsive de cards: 1×4 en >=1000px, 2×2 en <1000px. Sin mobile.

## Alternativas
- **Mobile-first** — rechazado: target es Tauri Windows, no hay touch.
- **Tamaño fijo sin responsive** — rechazado: usuarios redimensionan ventanas.
- **Soporte tablets** — rechazado v0.1: out of scope.

## Justificación
Tauri `tauri.conf.json` → `"minWidth": 900, "minHeight": 600, "width": 1280, "height": 800`. Tailwind breakpoints: `md:grid-cols-2 lg:grid-cols-4`. Vista detalle es 1 columna siempre.
