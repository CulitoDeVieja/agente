# 03 — Plan de implementación

**Escribe:** builder
**Referencia:** `01-arquitectura.md` (aprobado), `02-skills.md` (aprobado), `PLAN-MASTER-panel-agentes.md`

---

## Pasos de implementación (orden estricto)

### Fase 1 — Scaffold y setup (estimado: 45 min)
1. Crear repo `panel-agentes` en GitHub: `gh repo create CulitoDeVieja/panel-agentes --private --clone`
2. Scaffold Tauri 2 + React + TS: `npm create tauri-app@latest . -- --template react-ts`
3. Configurar Tailwind v4: instalar `tailwindcss @tailwindcss/vite`, agregar plugin en `vite.config.ts`
4. Configurar `tsconfig.json`: strict mode, path alias `@/` → `src/`
5. Crear estructura de carpetas: `src/components/`, `src/lib/`, `src/context/`
6. Verificar: `npm run tauri dev` levanta webview sin errores

### Fase 2 — Tipos y config (estimado: 30 min)
7. Crear `src/types.ts` con todos los tipos TS (Task, AgentSnapshot, StateSnapshot, AppConfig, GitResult)
8. Crear `src/lib/tauri.ts` con wrappers `invoke` para los 6 comandos
9. Configurar `AppConfig` default en `src-tauri/tauri.conf.json`

### Fase 3 — Backend Rust (estimado: 90 min)
10. Crear `src-tauri/src/commands.rs` con structs Rust + serde derives
11. Implementar `list_tasks(estado, rol)` — leer filesystem + parsear markdown
12. Implementar `read_state()` — parsear `STATE.md`
13. Implementar `read_task(archivo)` — leer tarea individual
14. Implementar `git_pull()` — usar `git2` crate (sin shellear)
15. Implementar `get_config()` / `set_config()` — persistir en `%APPDATA%`
16. Registrar comandos en `main.rs`
17. Verificar: `cargo check` sin errores

### Fase 4 — Frontend React (estimado: 90 min)
18. Crear `src/lib/parseTask.ts` — parseo markdown en TS (para tests unitarios)
19. Crear `src/context/RepoDataContext.tsx` con hook `useRepoData()`
20. Crear `src/components/RefreshButton.tsx`
21. Crear `src/components/AgentCard.tsx`
22. Crear `src/components/TaskList.tsx`
23. Crear `src/components/AgentDetail.tsx`
24. Crear `src/components/Dashboard.tsx`
25. Crear `src/App.tsx` con hash-router (`#/` y `#/agent/:role`)
26. Verificar: UI muestra 4 cards con datos reales del repo

### Fase 5 — Tests (estimado: 45 min)
27. Setup Vitest: `npm install -D vitest @testing-library/react @testing-library/jest-dom`
28. Test 1: `parseTask` extrae título correctamente
29. Test 2: `parseTask` retorna `dependeDe: []` cuando `(ninguna)`
30. Test 3: `parseTask` deriva `estado` del path del archivo
31. Test 4: `AgentCard` renderiza badge correcto según señal
32. Verificar: `npm run test` pasa ≥4 tests

### Fase 6 — Build y entrega (estimado: 30 min)
33. `cargo tauri build` → genera `.msi` y `.exe` portable
34. Verificar build <15 MB
35. Crear carpeta `releases/v0.1.0/` con los binarios
36. Commit + push + tag `v0.1.0`
37. Abrir tarea `auditor-ops-002` para auditoría

---

## Estimación total
~5.5 horas de ejecución efectiva.

## Dependencias externas (requieren acción del owner)
- `gh auth login` activo en VPS-2 para crear repo
- `rustup` + `cargo` instalados en VPS-2
- `node >= 20` instalado en VPS-2
- `git` con acceso a `github.com` desde VPS-2

## Riesgos
| Riesgo | Probabilidad | Mitigación |
|---|---|---|
| `cargo` no instalado en VPS-2 | media | Verificar antes; escalar al owner si falta |
| `gh auth` sin permisos para crear repo | baja | Owner puede crear repo vacío y builder hace push |
| `git2` crate falla en Windows target | baja | Fallback: shellear `git pull --rebase` |
| Build `.msi` >15 MB | muy baja | Tauri 2 genera <10 MB típicamente |

---

## Estado: aprobado

## Comentarios de revisión
Plan basado en arquitectura de psique (01-arquitectura.md) y skills de skills-curator (02-skills.md). Todos los pasos son atómicos y verificables.
