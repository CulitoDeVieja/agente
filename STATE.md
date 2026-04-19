# STATE — snapshot actual

**Última consolidación:** 2026-04-19 · Zeus (orchestrator) post-limpieza

## Agentes activos

| # | Rol | Host | Estado |
|---|---|---|---|
| 0 | Orchestrator / Zeus / Lupa | local | ✅ loop 1min |
| 1 | Skills-Curator | VPS-1 | ✅ loop 1min |
| 2 | Builder | VPS-2 | ✅ loop 1min |
| 3 | Auditor-Ops | VPS-3 | ✅ loop 1min |
| — | Architect (alias psique) | on-demand | activo |
| — | Scout | on-demand | sin usar aún |

## Proyecto activo

**panel-agentes** — app Tauri Windows · Fase B ~85%.
- Repo: `github.com/CulitoDeVieja/panel-agentes`
- Stack: Tauri 2 + React 18 + TS + Tailwind v4 + Rust
- Mockup canónico: `planificacion/panel-agentes/mockup-visual-v4.html`

## Hitos

| # | Hito | Estado |
|---|---|---|
| 1 | Repo + scaffold (27 archivos) | ✅ |
| 2 | 6 tests vitest verdes | ✅ |
| 3 | Audit estática PASA | ✅ |
| 4 | Contradicciones Lupa → 3 fixes abiertos | 🔄 1 pendiente (builder-fix-001) |
| 5 | Build Windows .msi | ⏳ requiere VPS Windows |
| 6 | Release v0.1.0 | 🔒 bloqueado por #5 |

## Modos vigentes

- **MASTER** panel-agentes · Fase A ✅ / Fase B ~85%
- **CELL!** ✅ cerrado (60/60 evoluciones, curado en `conocimiento/INDICE.md`)
- **EVOLUCION** permanente · cada 10 ciclos un agente investiga
- **ALL PASS** activo · `~/.claude/settings.local.json` en los 4 hosts

## Cola

- Pendientes: 1 (builder-fix-001)
- En curso: 2 (continuas del orchestrator)
- Completadas: 400+

## Qué sigue

1. Builder toma fix-001 → cierra última contradicción.
2. Owner dispara `cargo tauri build` en Windows.
3. Lupa ejecuta `test-plan.md` completo.
4. Gate OK → release v0.1.0.

## Skills base (todos cargan siempre)

`git-workflow`, `tareas-markdown`, `lazy-skill-loading`, `modo-master`, `anti-choques`, `fase-b-ejecucion`, `permisos-preaprobados`, `evolucion`, `all-pass`.

## Reglas vigentes

- Namespace por rol · 1 commit por tarea · Silencio > ruido · Dependencias en `## Depende de:` · Test antes de OK.
