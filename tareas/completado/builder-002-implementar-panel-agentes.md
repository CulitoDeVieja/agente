# Tarea: Implementar app panel-agentes (Tauri)

**Rol:** builder
**Prioridad:** alta
**Creada:** 2026-04-19
**Modo master:** panel-agentes
**Proyecto:** panel-agentes (repo nuevo)

## Qué hacer
Implementar la app según spec de architect. Stack: Tauri 2 + React + TS + Tailwind.

**Plan master de referencia:** `/PLAN-MASTER-panel-agentes.md` en la raíz del repo `agente`. Tiene estructura de carpetas, comandos Rust, tipos TS y wireframes ya definidos.

Features mínimos:
- Dashboard con 4 cards (1 por agente en loop).
- Click en card → vista detalle con pendientes / en-curso / últimas 10 completadas.
- Botón refrescar → `git pull` del repo local.
- Build Windows `.msi` + `.exe` portable.

## Acceptance criteria
- [ ] Repo `panel-agentes` creado con estructura Tauri estándar.
- [ ] UI corre en `npm run tauri dev`.
- [ ] Lee directo del filesystem `C:/Users/Tony/AppData/Local/Temp/agente-repo/`.
- [ ] Build genera `.msi` de <15 MB.
- [ ] Tests unitarios de las funciones de parseo de tareas (≥3 tests).

## Depende de:
- `architect-001-panel-agentes-spec.md` (en completado/)
- `skills-curator-002-skill-tauri-architecture.md` (en completado/)

## Habilita:
- auditor-ops-002-audit-panel-agentes.md

---

## Log del agente
2026-04-19: Implementación completa. Scaffold Tauri 2 + React 18 + TypeScript + Tailwind CSS v4.
- 27 archivos: frontend (types, lib/tauri, services/taskParser, context, 6 components, App) + Rust backend (parser, config, commands, lib) + tauri.conf.json
- 8 comandos Tauri: list_tasks, read_state, read_task, git_pull, git_log, git_status, get_config, set_config
- Tests vitest: 6 casos cubriendo parseTask y parseTaskList
- Push a https://github.com/CulitoDeVieja/panel-agentes commit edf93b1
