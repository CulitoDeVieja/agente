# Builder — Comprensión del plan completo v1

**Escribe:** builder (VPS-2)
**Fecha:** 2026-04-19
**Basado en:** PLAN-MASTER + 00..04 + 70 architect-detalle + 97 builder-detalle + mockup-v2 + skills

---

## 1. Orden de implementación (pasos sin ambigüedad)

### Bloque A — Infraestructura (pasos 1-9)
1. Crear repo `CulitoDeVieja/panel-agentes` en GitHub (`gh repo create`)
2. Scaffold Tauri 2 + React 18 + TypeScript + Vite (`npm create tauri-app@latest`)
3. Configurar Tailwind CSS v4 + `@tailwindcss/vite`
4. Configurar `tsconfig.json` strict + path alias `@/` → `src/`
5. Crear estructura de carpetas: `src/components/`, `src/lib/`, `src/context/`, `src/hooks/`, `src/services/`
6. Crear `src/types.ts` con todos los tipos (Task, AgentSnapshot, StateSnapshot, AppConfig, GitResult)
7. Crear `src-tauri/src/commands.rs` con structs Rust + serde
8. Configurar `tauri.conf.json` (productName, identifier, window size)
9. **CHECKPOINT 1**: `cargo check` + `npm run build` sin errores → commit `feat: scaffold + config`

### Bloque B — Backend Rust (pasos 10-17)
10. Implementar `list_tasks(estado, rol)` — leer filesystem + parsear markdown
11. Implementar `read_state()` — parsear STATE.md (tabla agentes + modo master badge)
12. Implementar `read_task(archivo)` — detalle tarea individual
13. Implementar `git_pull()` — usar crate `git2` (sin shellear)
14. Implementar `git_log(limit)` y `git_status()`
15. Implementar `get_config()` / `set_config()` — persist en `%APPDATA%/panel-agentes/config.json`
16. Registrar todos los comandos en `main.rs`
17. **CHECKPOINT 2**: `cargo check` limpio → commit `feat: rust commands`

### Bloque C — Services y lib TS (pasos 18-23)
18. Crear `src/lib/tauri.ts` — wrappers `invoke` para los 8 comandos
19. Crear `src/services/taskParser.ts` — parseTask + parseTaskList
20. Crear `src/services/configStore.ts` — get/set config
21. Crear `src/services/gitRunner.ts` — wrappea git_pull + git_status
22. Crear `src/services/fsReader.ts` — helpers filesystem
23. **CHECKPOINT 3**: `tsc --noEmit` limpio → commit `feat: services + lib ts`

### Bloque D — Context y hooks (pasos 24-30)
24. Crear `src/context/RepoDataContext.tsx` — AppProvider + useRepoData()
25. Crear `src/context/ConfigContext.tsx` — ConfigProvider + useConfig()
26. Crear `src/hooks/useTasks.ts`, `useAgents.ts`, `useGitStatus.ts`
27. Crear `src/hooks/usePolling.ts` — poll cada N ms, cleanup al desmontar
28. Crear `src/hooks/useNotifications.ts`, `useErrors.ts`, `useTheme.ts`
29. Crear `src/hooks/useShortcut.ts`, `useWindowSize.ts`
30. **CHECKPOINT 4**: hooks exportan correctamente → commit `feat: context + hooks`

### Bloque E — Componentes UI (pasos 31-45)
31. `RefreshButton.tsx` — spinner + estados idle/loading/ok/error
32. `AgentStatusBadge.tsx` — ✅/⏳/❌ por señal
33. `AgentCard.tsx` — card clickeable con contadores P/E/C
34. `TaskList.tsx` — lista reutilizable con maxItems + showBlocked
35. `TaskCard.tsx` — card individual con prioridad + 🔒 bloqueada
36. `AgentDetail.tsx` — vista detalle con 3 secciones (pendiente/en-curso/completado)
37. `Dashboard.tsx` — grid 2x2 con 4 AgentCard + header
38. `Header.tsx` — título + ModoMasterBadge + RefreshButton + botón ⚙
39. `SettingsModal.tsx` — form config repoPath/timeout/autoRefresh
40. `Toast.tsx`, `ErrorToast.tsx`, `SuccessToast.tsx`
41. `ConfirmDialog.tsx` — para acciones destructivas
42. `ErrorState.tsx`, `EmptyState.tsx`, `LoadingSkeleton.tsx`
43. `ErrorBoundary.tsx` — captura crashes React
44. `App.tsx` — hash-router (`#/` Dashboard, `#/agent/:role` AgentDetail)
45. **CHECKPOINT 5**: `npm run tauri dev` muestra 4 cards con datos reales → commit `feat: ui components`

### Bloque F — Tests (pasos 46-52)
46. Setup Vitest + Testing Library + jest-dom
47. Tests unitarios `taskParser.ts` (≥5 tests)
48. Tests unitarios hooks `useTasks`, `usePolling` (≥3 tests cada uno)
49. Tests unitarios services `configStore`, `gitRunner` (≥2 tests cada uno)
50. Test básico a11y con axe-core
51. Build debug local: verificar todas las features v0.1
52. **CHECKPOINT 6**: `npm run test` ≥15 tests passing → commit `test: unit + a11y`

### Bloque G — Release (pasos 53-57)
53. Crear icono app: `icon.png` 512x512 + `icon.ico` en `src-tauri/icons/`
54. `cargo tauri build` → `.msi` + `.exe` portable
55. Verificar build <15 MB, installer funciona
56. Crear `releases/v0.1.0/` con binarios + CHANGELOG.md
57. **CHECKPOINT 7**: tag `v0.1.0` + push + GitHub Release → commit `release: v0.1.0`

---

## 2. Dependencias entre componentes (grafo)

```
types.ts
  └── tauri.ts (lib)
        ├── taskParser.ts (service)
        │     └── RepoDataContext.tsx
        │           ├── useTasks.ts, useAgents.ts
        │           ├── AgentCard.tsx → Dashboard.tsx
        │           └── AgentDetail.tsx → TaskList.tsx → TaskCard.tsx
        ├── configStore.ts → ConfigContext.tsx → SettingsModal.tsx
        └── gitRunner.ts → RefreshButton.tsx → Header.tsx
App.tsx (hash-router) → Dashboard | AgentDetail
ErrorBoundary.tsx (wrappea App)
```

---

## 3. Checkpoints de commit + push

| # | Cuándo | Mensaje |
|---|---|---|
| 1 | Scaffold listo | `feat: scaffold Tauri2+React+TS+Tailwind` |
| 2 | Rust commands OK | `feat: rust commands (list_tasks, read_state, git_pull, config)` |
| 3 | Services TS OK | `feat: services + lib/tauri.ts` |
| 4 | Context + hooks OK | `feat: context RepoData + Config + hooks` |
| 5 | UI renderiza datos reales | `feat: componentes UI v0.1 funcional` |
| 6 | Tests passing | `test: unit parser/hooks/services + a11y` |
| 7 | Release build OK | `release: v0.1.0 .msi + .exe` |

---

## 4. Riesgos identificados al leer el plan completo

| Riesgo | Impacto | Plan |
|---|---|---|
| `cargo`/`rustup` no instalado en VPS-2 | Bloqueante | Verificar antes de empezar; pedir al owner si falta |
| `gh auth` sin permisos `repo` | Bloqueante | Owner puede crear repo vacío manualmente |
| `git2` crate falla en Windows cross-compile | Alto | Fallback: shellear `git` si git2 falla |
| Tailwind v4 + Vite incompatibilidad | Medio | Usar `@tailwindcss/vite` plugin oficial |
| Hash-router no persiste en webview Tauri | Bajo | `window.location.hash` es nativo, funciona |
| Build `.msi` >15 MB | Muy bajo | Tauri 2 genera ~8-12 MB típicamente |

---

## 5. Preguntas abiertas

1. **Ruta del repo en Windows**: El plan dice `C:/Users/Tony/AppData/Local/Temp/agente-repo/` — ¿el owner ya tiene el repo clonado ahí o el builder debe clonarlo también?
2. **Permisos `gh`**: ¿El token de GitHub en VPS-2 tiene permiso `repo` para crear `CulitoDeVieja/panel-agentes`?
3. **Tailwind v4 vs v3**: `02-skills.md` lista `tailwindcss@^4` pero algunos detalles de architect mencionan `@tailwind base` (directivas v3). ¿Confirmar cuál versión?

→ Escalando al orchestrator via tarea si no se resuelven antes de Fase B.

---

## Estado: aprobado (auto)
