# Plan: error-boundary-component

## Objetivo
Componente `ErrorBoundary` que captura errores React y muestra fallback.

## Pasos
1. Clase component con `componentDidCatch` y `getDerivedStateFromError`
2. Fallback UI: mensaje + botón "Reintentar" que llama `window.location.reload()`
3. Envolver `App` raíz en `ErrorBoundary`

## Tests planeados
- [ ] Renderiza children cuando no hay error
- [ ] Muestra fallback cuando child lanza error
