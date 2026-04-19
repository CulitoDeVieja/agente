# Bug report consolidado para Lupa — desde auditor-ops 3

**De:** auditor-ops 3 (psique)
**Para:** Lupa
**Fecha:** 2026-04-19
**Disparador:** owner pidió reporte completo
**Fuente verdad:** `notas/lupa/bugs.json`

---

## Resumen
- **Total:** 19 bugs
- **Cerrados:** 18
- **Abiertos:** 1 (severidad baja, no bloquea release)

## Mis aportes esta sesión (modo bug-hunter activo)

### Detectados por mí (auditor-ops via subagente + scan CI)
| ID | Sev | Bug | Estado |
|----|-----|-----|--------|
| LUPA-008 | alta | Backend serde snake_case vs frontend camelCase | ✅ cerrado (commit 520b70e) |
| LUPA-009 | alta | Build falla por `Task` import unused | ✅ cerrado |
| LUPA-010 | alta | Falta `src-tauri/capabilities/` | ✅ cerrado (commit 520b70e) |
| LUPA-011 | alta | `tauri.conf.json` referencia íconos inexistentes | ✅ cerrado (commit 520b70e) |
| LUPA-012 | media | README inconsistente (git2, pnpm) | ✅ cerrado (commit 70159ac) |
| LUPA-013 | media | `repo_path` default Windows-only | ✅ cerrado (commit 70159ac) |
| LUPA-014 | media | Falta `vitest.config` con jsdom | ✅ cerrado (commit 70159ac) |
| LUPA-015 | media | panel-agentes sin CI | ✅ cerrado (commit 520b70e) |
| LUPA-016 | media | CI rust-check: `cargo fmt --check` | ✅ cerrado (commit 280e5e8) |
| LUPA-017 | **alta** | `tauri::generate_context!` panicked — icon.png no RGBA | ✅ cerrado (commit 06a7d8c) |
| LUPA-018 | baja | Clippy: unused import `std::path::Path` | ✅ cerrado (commit 06a7d8c) |
| LUPA-019 | baja | Clippy: `manual_split_once` en parser.rs:77 | 🔄 abierto (builder-fix-014 ya cerrada por builder, esperando CI run) |

### Resueltos por mí directamente (modo OPS)
- **LUPA-005** (alta) default branch ops/init → main: hecho via `gh api PATCH default_branch=main` + DELETE ops/init.
- **LUPA-007** (media) auditor-ops usa `git mv` no `rm`: confirmado patrón en uso desde primer commit.
- **LUPA-015** (media) intenté setup CI workflow, push bloqueado por scope `workflow` faltante; Zeus lo terminó.

## Único bug abierto

**LUPA-019** — clippy `manual_split_once` en `src-tauri/src/parser.rs:77`. Severidad baja. Tarea `builder-fix-014` ya cerrada por builder (commit `b1a7986`), esperando CI run `24635628736` que está IN_PROGRESS. Si pasa → cierro yo.

## Implicación gate de release v0.1
Una vez LUPA-019 cierre y CI quede verde → **0 bugs abiertos**. Único bloqueante restante: build Windows real (.msi) que requiere VPS Windows.

## Loop activo
Cron `e6a7f113` (1min) sigue scaneando. Cualquier bug nuevo en CI o configs → tarea + reporte automático.

— psique
