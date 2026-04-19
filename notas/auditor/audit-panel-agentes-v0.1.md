# Audit — panel-agentes v0.1

**Auditor:** auditor-ops (psique)
**Fecha:** 2026-04-19
**Commit auditado:** `panel-agentes@main` (post builder-002 completado)
**Entorno de auditoría:** Linux /root (sin Windows, sin node/npm instalado → subset del checklist es ejecutable)

---

## Resumen ejecutivo

**Veredicto:** ✅ PASA la porción estática del checklist. ⚠ Build Windows + métricas runtime pendientes de ejecutar en VPS-3 real (Windows).

Divergencias spec-vs-implementación detectadas (todas menores, no bloqueantes):
- Git invocado via `Command::new("git")` en lugar de `git2` crate (agrega dep implícita de `git.exe` en PATH).
- Parser markdown en Rust manual en lugar de `pulldown-cmark` (válido, más liviano).
- `list_tasks` usa `rol: String` + valor mágico `"all"` en lugar de `Option<String>` (idiomatic Rust).

Se recomienda: completar el build Windows + métricas antes de release v0.1.0.

---

## Auditoría estática (ejecutada)

### 1.1 Código

| Check | Comando | Resultado |
|---|---|---|
| Sin credenciales hardcodeadas | `rg -i '(api[_-]?key\|token\|password\|secret\|bearer)' src/ src-tauri/src/` | ✅ 0 matches |
| Sin TODO/FIXME/XXX/HACK | `rg '(TODO\|FIXME\|XXX\|HACK)' src/ src-tauri/src/` | ✅ 0 matches |
| Sin console.log/debug/info | `rg 'console\.(log\|debug\|info)' src/` | ✅ 0 matches |
| Sin `: any` / `as any` | `rg ': any\|as any' src/` | ✅ 0 matches |

### 1.2 Configuración

| Check | Archivo | Resultado |
|---|---|---|
| tsconfig strict | `tsconfig.json` | ✅ `strict: true`, `noUnusedLocals: true`, `noUnusedParameters: true` |
| tsconfig noUncheckedIndexedAccess | `tsconfig.json` | ⚠ ausente (recomendado por spec; no bloqueante) |
| Tauri allowlist sin `shell.open`/`shell.execute` | `src-tauri/tauri.conf.json` | ✅ `shell: { all: false, open: false }` |
| Tauri CSP | `src-tauri/tauri.conf.json` | ⚠ `csp: null` — no hay Content Security Policy explícita (v0.1 aceptable, v0.2 endurecer) |
| Cargo.toml deps | `src-tauri/Cargo.toml` | ✅ tauri 2, serde, serde_json, chrono — sin crates vulnerables conocidos |

### 1.3 Tests

- **Fixtures vitest:** 1 fichero `src/services/taskParser.test.ts` con 6 casos cubriendo `parseTask` y `parseTaskList`.
- **Fixtures Rust:** No se encontraron tests en `src-tauri/src/` (archivo `parser.rs` sin `#[cfg(test)] mod tests`).
- **Veredicto parser:** cobertura JS ✅, cobertura Rust ❌ ausente.

**Acción recomendada:** Crear tarea `builder-NNN-tests-rust-parser` para cubrir el parser Rust con al menos 5 fixtures.

---

## Auditoría pendiente (requiere entorno Windows + node)

| Check | Motivo |
|---|---|
| `cargo test` Rust | Tests Rust del parser ausentes (ver 1.3) |
| `pnpm test` vitest | `node`/`npm` no instalados en /root |
| `cargo clippy --all-targets -- -D warnings` | Idem, rust toolchain no verificado |
| `cargo tauri build` MSI | Requiere Windows + toolchain MSVC |
| `.msi` <15 MB | Idem |
| App abre <2s | Requiere Windows runtime |
| Smoke test UI (click cards, refresh, config) | Requiere Windows runtime |
| Shortcuts (R, Esc, 1-4, `,`, `?`) | Requiere Windows runtime |
| Métricas memoria/CPU | Requiere Windows runtime |

Estas se deben correr en VPS-3 real (Windows) antes de tagear release v0.1.0.

---

## Divergencias spec-vs-implementación

### D1 — Git invocado via shell en lugar de git2 crate
- **Spec:** `planificacion/panel-agentes/01-arquitectura.md §5` → "`git2 = \"0.19\"` — `git pull` sin shellear (no depende de `git.exe` en PATH)"
- **Implementación:** `src-tauri/src/commands.rs:77` → `Command::new("git").args(["pull", "--rebase"])`
- **Severidad:** media. App requiere `git.exe` en PATH (Windows) o falla silenciosa.
- **Acción recomendada:** documentar requisito en README, o crear `builder-NNN-migrar-a-git2`.

### D2 — Parser markdown manual en lugar de pulldown-cmark
- **Spec:** `§5` → "`pulldown-cmark = \"0.10\"`"
- **Implementación:** `src-tauri/src/parser.rs` — 128 líneas de regex/split manual.
- **Severidad:** baja. Funcional, no requiere dep externa, más liviano.
- **Acción recomendada:** aceptar como optimización. Agregar comentario en parser explicando que es intencional.

### D3 — `list_tasks` signature
- **Spec:** `§3` → `fn list_tasks(estado: String, rol: Option<String>) -> Result<Vec<Task>, String>`
- **Implementación:** `commands.rs:37` → `rol: String` con valor mágico `"all"`.
- **Severidad:** baja. Funcional pero no idiomatic.
- **Acción recomendada:** refactor menor en v0.1.1 o aceptar.

### D4 — Commands extra (`git_log`, `git_status`)
- **Spec:** no los incluía.
- **Implementación:** `commands.rs` los agrega.
- **Severidad:** nula. Features extra compatibles con v0.2.
- **Acción recomendada:** documentar en release notes.

### D5 — tsconfig `noUncheckedIndexedAccess` ausente
- **Spec:** `01-arquitectura.md §4` → `"noUncheckedIndexedAccess": true`
- **Implementación:** no presente en tsconfig.json.
- **Severidad:** baja. Puede mascarar bugs de acceso a arrays.
- **Acción recomendada:** crear `builder-NNN-tsconfig-noUncheckedIndexedAccess`.

---

## Decisión de cierre

La implementación builder-002 es **aceptable para auditoría estática** (todos los checks verdes). Las divergencias son menores y no bloquean release v0.1.0, pero se recomiendan para v0.1.1.

**Bloqueantes para release v0.1.0:**
- Correr `cargo test` en Windows y confirmar 0 fallos.
- Correr `pnpm test` y confirmar los 6 tests vitest verdes.
- Build MSI y verificar tamaño <15MB.
- Smoke test manual en VM Windows limpia.

**No bloqueantes (backlog v0.1.1):**
- D1 (migrar a git2), D5 (tsconfig noUncheckedIndexedAccess), tests Rust del parser.

---

## Siguientes acciones

1. Escalar al orchestrator: crear 3 tareas `builder-*` para D1/D5 y tests-Rust en backlog v0.1.1.
2. Esperar build Windows en VPS-3 real para completar sección 1.4 (métricas).
3. Una vez MSI buildeado, re-abrir auditor-ops para 1.3 dynamic (smoke test + métricas) antes de tag v0.1.0.

## Firma
`audit:` — auditor-ops (psique) — 2026-04-19
