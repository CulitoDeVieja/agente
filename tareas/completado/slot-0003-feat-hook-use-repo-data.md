# Slot 0003 · feat · hook-use-repo-data

**Fase:** 1 · Data layer
**Libre para tomar:** bot con skill React hooks
**Asignado:** (pendiente)
**Rama:** `slot-0003/use-repo-data`

## Contexto

React hook que combina `fetchTareasList` (slot-0001) + `parseTaskMarkdown` (slot-0002) para alimentar la UI. Modo PWA sin Tauri.

## Objetivo

Reemplazar el `RepoDataContext` actual (que invoca Tauri) por uno que use fetch HTTP al repo GitHub.

## Archivo a modificar/crear

`panel-agentes/src/context/RepoDataContext.tsx` (ya existe, adaptar)

## Firma esperada

```ts
type RepoData = {
  tasks: TaskParsed[];
  byRole: (role: string) => TaskParsed[];
  refresh: () => Promise<void>;
  loading: boolean;
  error: string | null;
  lastFetch: Date | null;
};

export function useRepoData(): RepoData;
export function RepoDataProvider({ children }: { children: ReactNode }): JSX.Element;
```

## Lógica

1. Al montar → `fetchTareasList()` → por cada archivo, fetch raw content → `parseTaskMarkdown()`.
2. Guardar array en state.
3. `refresh()` → repetir.
4. Cache 30s para evitar rate-limit GitHub API.
5. Si Tauri está disponible (`window.__TAURI_INTERNALS__`), preferir IPC nativo (fallback dual).

## AC

- [ ] Context + hook creado/adaptado.
- [ ] Detección Tauri vs web correcta.
- [ ] Loading + error states expuestos.
- [ ] Tests: 3 casos (web mode, Tauri mode, refresh).

## Depende de:
- `slot-0001-feat-fetch-tareas-github-raw.md` (completado/)
- `slot-0002-feat-parse-task-markdown-ts.md` (completado/)

## Habilita
- slot-0004 (Dashboard con datos reales)
- Toda la UI posterior


## Log del agente (builder_loop)

**Builder:** pc-tv + Qwen 2.5 Coder 7B
**Archivos escritos:** ['src\\context\\RepoDataContext.tsx']
**Tests:** PASS (87.3s de Qwen)
**Merge:** en `main` de panel-agentes

**Commit:** `17ab12b`
