# Plan: componente-refresh-button

## Objetivo
Botón `↻` que ejecuta `git_pull()` y recarga datos con feedback visual.

## Props
```ts
type RefreshButtonProps = { onRefresh: () => Promise<void>; }
```

## Estados visuales
- idle: `↻ Refrescar`
- loading: spinner + `Actualizando...`
- ok: `✅ Actualizado` (2s luego vuelve a idle)
- error: `❌ Error` con tooltip mensaje

## Tests planeados
- [ ] Muestra spinner durante loading
- [ ] Vuelve a idle tras éxito
