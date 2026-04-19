# Tarea: Auditar panel-agentes + producir `.exe` final

**Rol:** auditor-ops
**Prioridad:** alta
**Creada:** 2026-04-19
**Modo master:** panel-agentes

## Qué hacer
Modo AUDITOR:
- Correr los tests del Builder. Deben pasar todos.
- Verificar que el UI muestra las 4 cards y que el refrescar funciona.
- Revisar que no hay credenciales hardcodeadas.

Modo OPS:
- Build final `.msi` + `.exe` portable.
- Publicarlo en `panel-agentes/releases/v0.1.0/`.

## Acceptance criteria
- [ ] Tests verdes. (pendiente — requiere node/npm en VPS-3 Windows; estática ok)
- [x] Audit report en `notas/auditor/audit-panel-agentes-v0.1.md`.
- [ ] `.msi` y `.exe` subidos al release v0.1.0. (bloqueado — requiere Windows build)

## Depende de:
- `builder-002-implementar-panel-agentes.md` (en completado/)

## Habilita:
(fin de modo master)

---

## Log del agente
- Audit report completo: `notas/auditor/audit-panel-agentes-v0.1.md`
- **Auditoría estática: ✅ PASA**
  - Sin credenciales hardcodeadas (0 matches)
  - Sin TODO/FIXME/XXX/HACK (0 matches)
  - Sin `console.log/debug/info` (0 matches)
  - Sin `: any` / `as any` (0 matches)
  - tsconfig `strict: true`, `noUnusedLocals`, `noUnusedParameters` ✅
  - Tauri allowlist: `shell: { all: false, open: false }` ✅
- **Auditoría pendiente (requiere Windows + node en VPS-3):**
  - `cargo test`, `pnpm test`, `cargo tauri build`, smoke UI, métricas.
- **Divergencias spec-vs-implementación (no bloqueantes):**
  - D1: git via `Command::new("git")` vs `git2` crate (spec) — requiere `git.exe` en PATH.
  - D2: parser markdown manual vs pulldown-cmark (válido).
  - D3: `list_tasks(rol: String)` con "all" vs `Option<String>` (spec).
  - D4: commands extra git_log/git_status (bienvenidos).
  - D5: tsconfig `noUncheckedIndexedAccess` ausente.
- **Decisión:** release v0.1.0 NO tageable hasta que VPS-3 Windows corra tests + build. 3 tareas backlog v0.1.1 recomendadas (git2, noUncheckedIndexedAccess, tests-Rust-parser).
- Tarea `auditor-ops-002` cerrada parcialmente: audit report entregado. Release + smoke queda para VPS-3.
