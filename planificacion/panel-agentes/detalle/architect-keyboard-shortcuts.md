# Decisión: Keyboard shortcuts

**Decisión:** 4 shortcuts v0.1: `R` (refresh), `Esc` (back a Dashboard), `1-4` (navegar a cada agente card), `,` (abrir config). Registrados vía `window.addEventListener("keydown")`.

## Alternativas
- **Sin shortcuts** — rechazado: app de cockpit, power user.
- **Biblioteca `react-hotkeys-hook`** — rechazado: 4 shortcuts no justifican dep.
- **Global shortcuts del sistema (Tauri global shortcut plugin)** — rechazado v0.1: con app en foreground basta.

## Justificación
Navegación de teclado es free para un dashboard. `useEffect` con `addEventListener/removeEventListener` en `<App>` raíz cubre el caso. Input fields (config modal) frenan propagación con `e.stopPropagation()`.
