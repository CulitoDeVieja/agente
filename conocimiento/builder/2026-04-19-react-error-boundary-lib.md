# react-error-boundary vs Error Boundary built-in

## TL;DR
- React nativo: Error Boundary **solo en class component** (requiere `componentDidCatch`/`getDerivedStateFromError`). Captura render/lifecycle/constructors — NO event handlers ni async.
- **`react-error-boundary`** (lib): componente `<ErrorBoundary>` para functional, hook `useErrorBoundary`, fallback flexible, reset automático. Es el estándar de facto 2026.
- React 19: Actions + `useTransition` propagan errores al boundary más cercano automáticamente.

## Detalles

### Instalación
```bash
bun add react-error-boundary
```

### Patrón declarativo
```tsx
import { ErrorBoundary } from 'react-error-boundary'

<ErrorBoundary
  FallbackComponent={ErrorFallback}
  onReset={() => queryClient.invalidateQueries()}
  onError={(err, info) => logToRust(err, info)}
>
  <AgentList />
</ErrorBoundary>
```

### Errores en event handlers / async
Error boundary nativo NO los captura. Solución del lib:
```tsx
const { showBoundary } = useErrorBoundary()
async function handleClick() {
  try { await invoke('delete_agent', { id }) }
  catch (e) { showBoundary(e) }
}
```

### React 19 automático
```tsx
// useTransition + action: error cae al boundary sin try/catch
const [pending, start] = useTransition()
start(async () => { await deleteAgent(id) })
```

### Qué poner en el FallbackComponent
- Título humano: "Algo falló al leer los agentes"
- Botón "Reintentar" → `resetErrorBoundary()`
- Debug info solo en dev (`import.meta.env.DEV`)
- Opcional: abrir issue con `@tauri-apps/plugin-shell` → GitHub

### Jerarquía recomendada en panel-agentes
```
<ErrorBoundary> (app-level, fallback full-screen)
  <ErrorBoundary> (feature-level, fallback inline)
    <AgentTable />
```
Granularidad = aislar fallos por feature sin tirar toda la app.

## Fuente
- https://www.npmjs.com/package/react-error-boundary
- https://www.epicreact.dev/why-react-error-boundaries-arent-just-try-catch-for-components-i6e2l
- https://weblogtrips.com/programming-languages/web-development/react-suspense-error-boundaries-modern-guide-2026/

## Aplicabilidad a panel-agentes
**Sí, adoptar `react-error-boundary`.** Con Tauri+invoke, los comandos Rust pueden fallar por IO/permisos/git. Necesitamos:
1. Boundary app-level en `App.tsx` → fallback de emergencia + botón "recargar ventana" (`window.location.reload()`).
2. Boundary por panel (AgentTable, LogsViewer, StateViewer) → aísla fallos.
3. `onError` → log a `src-tauri/logs/frontend-errors.log` vía comando Rust para debug.
4. Combinar con Suspense de `use()` sobre invoke → pattern unificado loading+error.
