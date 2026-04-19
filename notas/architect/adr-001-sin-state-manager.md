# ADR-001 — Sin state manager, usar React Context

**Fecha:** 2026-04-19
**Proyecto:** panel-agentes
**Autor:** architect (psique)
**Estado:** aceptado

## Contexto
La app tiene 2 vistas (Dashboard + Detalle) y un único "dataset" global (STATE + tareas). Refresco es manual (botón ↻) y opcionalmente on-focus. No hay mutaciones desde la UI en v0.1.

## Decisión
Usar `React.createContext` con un solo provider (`<RepoDataProvider>`) que expone `{ state, tasks, refresh, lastPull, isLoading }`. Segundo provider (`<ConfigProvider>`) para rutas/timeouts.

**Ningún state manager externo** (Redux, Zustand, Jotai, Valtio, MobX).

## Alternativas evaluadas
- **Zustand**: 1KB, hooks ergonómicos. Pero una sola store global no justifica la dep cuando `useContext` basta.
- **Redux Toolkit**: reducers + middleware overkill, 12KB+ en bundle.
- **React Query / SWR**: razonable si el dataset fuese servido por HTTP. Como leemos filesystem vía comandos Tauri y el refresco es manual, no aporta cache management útil.

## Consecuencias
- **Positivas:**
  - Menos superficie de dependencias.
  - Builder no necesita aprender lib extra.
  - Bundle JS más chico (métrica `.msi <15MB` ayuda).
- **Negativas:**
  - Re-renders más amplios si el context cambia muy seguido. Mitigar:
    - Split en dos contexts (`RepoDataContext` vs `ConfigContext`).
    - Memorizar componentes hijos que solo leen parte del estado.
  - Si en v0.2 agregamos mutaciones con estado optimista, revisar esta decisión (re-abrir ADR).

## Gatillos para reconsiderar
- Si aparecen >3 contexts que se anidan.
- Si se introducen mutaciones optimistas con rollback.
- Si refresco pasa a ser automático por polling rápido (<5s) y causa re-render storm.
