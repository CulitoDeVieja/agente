# Plan: context-global-state

## Objetivo
Crear React Context global con estado de agentes y tareas.

## Estructura
```ts
type AppState = {
  agents: AgentSnapshot[];
  tasks: Task[];
  modoMaster: boolean;
  loading: boolean;
  lastRefresh: Date | null;
  refresh: () => Promise<void>;
};
```

## Pasos
1. Crear `src/context/AppContext.tsx`
2. Provider llama `read_state()` y `list_tasks("all", "all")` al montar
3. `refresh()` ejecuta `git_pull()` luego recarga
4. Exponer hook `useAppState()`

## Tests planeados
- [ ] `useAppState()` fuera de Provider lanza error descriptivo
- [ ] `loading` true durante fetch
