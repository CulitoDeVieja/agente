# react-router-setup

## Propósito
Configurar React Router v6 en una app Vite + React + TypeScript.

## Cuándo usar
Al agregar navegación multi-vista a una app React.

## Pasos
1. Instalar: `npm install react-router-dom`
2. Envolver app en `main.tsx`:
   ```tsx
   import { BrowserRouter } from "react-router-dom";
   <BrowserRouter><App /></BrowserRouter>
   ```
3. Definir rutas en `App.tsx`:
   ```tsx
   import { Routes, Route } from "react-router-dom";
   <Routes>
     <Route path="/" element={<Dashboard />} />
     <Route path="/agent/:id" element={<AgentDetail />} />
   </Routes>
   ```
4. Navegar: `const navigate = useNavigate(); navigate("/agent/builder")`

## Ejemplo
```tsx
import { useParams } from "react-router-dom";
function AgentDetail() {
  const { id } = useParams<{ id: string }>();
  return <h1>{id}</h1>;
}
```

## Errores comunes
- En Tauri usar `MemoryRouter` en lugar de `BrowserRouter` si las rutas fallan en build.
- `useNavigate` fuera de `Router` → error; verificar que el proveedor está en `main.tsx`.
- Rutas dinámicas no coinciden → verificar que el parámetro usa `:nombre` exacto.
