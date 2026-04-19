# Decisión: State management

**Decisión:** React Context con 2 providers separados (`ConfigProvider` + `RepoDataProvider`). Sin lib externa. Ver ADR-001.

## Alternativas
- **Zustand** — 1KB, hooks limpios; descartado porque un solo dataset no justifica dep.
- **Redux Toolkit** — 12KB+, reducers/middleware innecesarios.
- **Jotai / Valtio / MobX** — válidos pero resuelven problemas que no tenemos.
- **React Query / SWR** — útiles para HTTP; filesystem via comandos Tauri no tiene cache invalidation compleja.

## Justificación
1 dataset global + 1 config global, ambos con refresh manual. `useContext` cubre el caso. Splitear en 2 contexts limita re-renders (config cambia rara vez). Re-abrir ADR si en v0.2 aparecen mutaciones optimistas.
