# cmdk — command menu ⌘K estilo Raycast/Linear

## TL;DR
- **cmdk** (Paco Coursey) = componente React unstyled para command palette ⌘K. Usado por Vercel, Linear, Raycast.
- Filtrado fuzzy built-in (`command-score`), keyboard nav, grouping, composición vía data-attrs.
- Sin virtualización: funciona bien hasta ~2000-3000 items.

## Detalles

### Install
```bash
bun add cmdk
# o vía shadcn
bunx shadcn@latest add command
```

### Estructura
```tsx
import { Command } from 'cmdk'

<Command label="Panel Agentes">
  <Command.Input placeholder="Buscar acción..." />
  <Command.List>
    <Command.Empty>Sin resultados.</Command.Empty>
    <Command.Group heading="Agentes">
      <Command.Item onSelect={() => restart(1)}>Reiniciar agente 1</Command.Item>
      <Command.Item onSelect={() => restart(2)}>Reiniciar agente 2</Command.Item>
    </Command.Group>
    <Command.Group heading="Repo">
      <Command.Item onSelect={() => pull()}>Git pull</Command.Item>
      <Command.Item onSelect={() => openState()}>Abrir STATE.md</Command.Item>
    </Command.Group>
  </Command.List>
</Command>
```

### Dialog overlay (patrón estándar ⌘K)
```tsx
<Command.Dialog open={open} onOpenChange={setOpen} label="Global">
  <Command.Input />
  <Command.List>...</Command.List>
</Command.Dialog>
```

### Hook para ⌘K global
```tsx
useEffect(() => {
  const down = (e: KeyboardEvent) => {
    if (e.key === 'k' && (e.metaKey || e.ctrlKey)) {
      e.preventDefault()
      setOpen(o => !o)
    }
  }
  document.addEventListener('keydown', down)
  return () => document.removeEventListener('keydown', down)
}, [])
```

### Styling
Data-attrs: `[cmdk-root]`, `[cmdk-input]`, `[cmdk-item]`, `[cmdk-group-heading]`, `[cmdk-item][data-selected="true"]`. Tailwind fácil con selectors o usar shadcn/ui que trae estilos.

### Custom scoring
```tsx
<Command filter={(value, search) => value.includes(search) ? 1 : 0}>
```

## Fuente
- https://github.com/pacocoursey/cmdk
- https://www.shadcn.io/ui/command
- https://reactscript.com/command-k-menu/

## Aplicabilidad a panel-agentes
**Sí, feature killer.** Tony va a tener 3+ agentes, múltiples tareas, commits, logs. Con ⌘K podés:
- Buscar agente por número/estado
- Ejecutar acciones sin navegar (restart, kill, open logs)
- Abrir archivos clave (STATE.md, tareas pendientes)
- Buscar tareas por nombre/prefijo

Plan: instalar vía shadcn (`bunx shadcn add command`), montar `<CommandPalette />` en layout, hook global ⌘K. Fuentes de comandos: estáticos (agentes, repo actions) + dinámicos (items de pendiente/en-curso). Perfect match para un panel de productividad.
