# Plan: router-app

## Objetivo
Configurar routing con React Router en `App.tsx`.

## Rutas
- `/` → `Dashboard`
- `/agent/:role` → `AgentDetail`

## Pasos
1. `npm install react-router-dom`
2. Envolver app en `<BrowserRouter>` (o `<MemoryRouter>` para Tauri)
3. Definir rutas en `App.tsx`
4. Usar `<MemoryRouter>` porque Tauri no tiene servidor HTTP

## Tests planeados
- [ ] Navegar a `/agent/builder` renderiza `AgentDetail` con role correcto
- [ ] Ruta inexistente muestra fallback
