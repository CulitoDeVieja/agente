# Plan: hook-use-repo-data

## Objetivo
Hook `useRepoData()` que encapsula llamadas a comandos Tauri.

## API
```ts
function useRepoData(): {
  state: StateSnapshot | null;
  tasks: Task[];
  loading: boolean;
  error: string | null;
  reload: () => Promise<void>;
}
```

## Pasos
1. `useEffect` inicial llama `read_state()` y `list_tasks("all","all")`
2. `reload()` ejecuta `git_pull()` luego recarga
3. Manejo de errores con `error` string

## Tests planeados
- [ ] `loading` true al montar, false al completar
- [ ] `error` se setea si comando Tauri falla
