# Decisión: Rutas de la app

**Decisión:** Hash routing nativo (`location.hash`) con 2 rutas: `#/` (Dashboard) y `#/agent/:role` (Detalle).

## Alternativas
- **react-router-dom** — rechazado: 10KB+ de bundle para 2 rutas estáticas.
- **BrowserRouter (history API)** — rechazado: Tauri sirve desde `tauri://localhost`; history API funciona pero hash es más simple.
- **Sin routing, state `view`** — rechazado: pierde back button y URL share (aunque no aplique, da hábito).

## Justificación
Hash routing en Tauri es 10 líneas de código (`useEffect` + `hashchange` listener), cero deps, back/forward funciona. Para 2 rutas sobra.
