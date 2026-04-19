# Sonner — toasts para React

## TL;DR
- **Sonner** (Emil Kowalski) es el estándar de facto para toasts en React 2026. 30M+ downloads/semana, usado por Cursor, X, Vercel.
- API simple, **toast.promise()** para async, stacking animation icónico, accesible (ARIA live).
- Manejo de estado vía Observer Pattern (no Context) → funciona fuera del árbol React.

## Detalles

### Install
```bash
bun add sonner
```

### Setup único
```tsx
// App.tsx
import { Toaster } from 'sonner'
return <>
  <Toaster position="bottom-right" richColors closeButton />
  <Router />
</>
```

### Uso
```ts
import { toast } from 'sonner'

toast('Agente 2 reiniciado')
toast.success('Tarea completada')
toast.error('Falló el push al repo')
toast.warning('Cola con >50 items')
toast.info('Orchestrator verificó 22 tareas')
```

### Promise API (estrella)
```ts
toast.promise(
  invoke('restart_agent', { id: 2 }),
  {
    loading: 'Reiniciando agente...',
    success: (data) => `Agente ${data.id} activo`,
    error: (err) => `Falló: ${err.message}`,
  }
)
```
Se encarga de los 3 estados automáticamente + timeout inteligente.

### Custom con action
```ts
toast('Commit revertido', {
  action: { label: 'Rehacer', onClick: () => redoCommit() },
})
```

### Rich content
`toast.custom((id) => <MyJSX onClose={() => toast.dismiss(id)} />)`

### shadcn integration
```bash
bunx shadcn@latest add sonner
```
Genera `components/ui/sonner.tsx` con theme integrado (dark/light).

### Por qué no usar Radix Toast
Radix Toast requiere boilerplate (provider + hook). Sonner = 1 componente + función global. DX superior para panel-agentes.

## Fuente
- https://github.com/emilkowalski/sonner
- https://sonner.emilkowal.ski/
- https://ui.shadcn.com/docs/components/radix/sonner

## Aplicabilidad a panel-agentes
**Sí, default para notifications.** Patrón ideal:
- `toast.promise()` envolvente de cada `invoke()` Tauri → feedback inmediato sobre acciones del usuario (restart, kill, commit, push).
- `<Toaster richColors closeButton position="bottom-right" />` en App root.
- Config `theme="system"` → respeta preferencia OS.
- Wrapper `src/lib/toast.ts` centraliza estilos y evita acoplar componentes a sonner directo (por si cambiamos).
