# Audit dinámico panel-agentes — handoff a Zeus

**De:** auditor-ops 3 (psique)
**Para:** Zeus / orchestrator
**Fecha:** 2026-04-19
**Disparador:** owner pidió "buscá bugs y resolvelos que todo funcione bien"

## Método
Spawn 1 subagente general-purpose para audit dinámico del repo `panel-agentes` (clone + inspect estático del código y configs). El VPS no tiene node/cargo/npm — auditoría 100% estática, sin runs.

## Hallazgos — 8 bugs nuevos (4 ALTAS + 3 medias + 1 ops media)

| ID | Sev | Bug | Tarea creada |
|---|---|---|---|
| LUPA-008 | ALTA | Backend snake_case vs frontend camelCase → MODO MASTER nunca aparece | builder-fix-004-serde-camelcase |
| LUPA-009 | ALTA | Build release falla por `Task` import unused (noUnusedLocals) | builder-fix-005-import-unused |
| LUPA-010 | ALTA | Falta `src-tauri/capabilities/` → commands custom no invocables (panel queda en blanco) | builder-fix-006-tauri-capabilities |
| LUPA-011 | ALTA | `tauri.conf.json` referencia íconos inexistentes → `cargo tauri build` falla | builder-fix-007-tauri-icons |
| LUPA-012 | media | README dice git2 (rechazado en ADR-001) y pnpm (usa npm) | builder-fix-008-readme-deps |
| LUPA-013 | media | `repo_path` default es path Windows-only | builder-fix-009-config-default-cross-platform |
| LUPA-014 | media | Sin `vitest.config` con `environment: jsdom` | builder-fix-010-vitest-jsdom |
| LUPA-015 | media | panel-agentes sin `.github/workflows/` | auditor-ops-fix-004-setup-ci-workflow (mía, ops) |

## Implicancia para gate de release
- Lupa había firmado audit estática + 7/7 tests = OK.
- **Pero el build release está ROTO** (LUPA-009 + LUPA-011) y el runtime quedaría en blanco (LUPA-010, LUPA-008).
- Gate v0.1 NO debe firmarse hasta que builder cierre LUPA-008..011 (4 ALTAS). Las 4 medias pueden ir a v0.1.1 si se prefiere.

## Lo que tomé yo (auditor-ops, scope ops)
- `auditor-ops-fix-004-setup-ci-workflow` — pendiente, la voy a procesar en próximo firing del cron (1min). Crea `.github/workflows/ci.yml` en panel-agentes con frontend job + backend Rust job + cargo-audit.

## Lo que necesito de vos (Zeus)
1. **Aprobar/rechazar** las 7 tareas builder-fix-* creadas (004..010).
2. **Refrescar STATE.md**: actualmente dice "1 pendiente (builder-fix-001)" pero ya está cerrado, y ahora hay 8 nuevas pendientes.
3. **Decidir scope v0.1**: ¿bloqueamos por las 4 ALTAS o reclasificamos algunas?
4. Si querés que tome más bugs en scope auditor-ops (más allá de los obviamente ops), avisame y los proceso.

## bugs.json actualizado
`notas/lupa/bugs.json` — total 15, cerrados 7, abiertos 8 (4 ALTAS + 4 medias).

— psique
