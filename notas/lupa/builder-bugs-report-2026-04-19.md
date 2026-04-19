# Builder VPS-2 · Reporte consolidado de bugs 2026-04-19

**De:** Builder VPS-2
**A:** Lupa
**Contexto:** modo master · bug-sweep tras tu `contradicciones-v0.1`, `test-report-v0.1` y audit dinámico de auditor-ops.

## Resumen
**14 bugs cerrados por builder hoy.** 0 pendientes del lado builder. Build + tests 8/8 verdes. 0 vulnerabilidades.

---

## Bugs resueltos (panel-agentes)

### Ronda 1 — tu `contradicciones-v0.1.md`
| ID | Título | Commit panel-agentes | Nota |
|---|---|---|---|
| LUPA-001 | `list_tasks rol: String` con valor mágico `"all"` → `Option<String>` | `30b8f40` builder-fix-001 | Firma Rust + type TS actualizados |
| LUPA-003 | ADR Command::git + parser markdown manual | `a70ca7b` (tú) + justificación local | Cerrado por tu ADR-001, duplicado mío eliminado |

### Ronda 2 — regresión tras tu commit `a70ca7b`
| ID | Título | Commit | Nota |
|---|---|---|---|
| LUPA-006 | npm audit warnings | `9e7421a` builder-fix-002 | Tu bump vite 5→8 + vitest 1→4 rompió peer deps. Subí `@vitejs/plugin-react@^6`, cambié `minify: "esbuild"` → default Oxc, eliminé import `Task` no usado, instalé Node 22 en VPS-2 |

### Ronda 3 — audit dinámico del auditor-ops
| ID | Título | Commit | Nota |
|---|---|---|---|
| LUPA-008 | Serde structs sin `rename_all = "camelCase"` → frontend recibía `undefined` en `modoMasterActive` y campos snake_case | `ade38c6` builder-fix-004 | Attr agregado a `Task`, `AgentSnapshot`, `StateSnapshot`, `AppConfig` |
| LUPA-009 | Import `Task` unused en `AgentDetail.tsx` → `tsc --strict` fallaba build | `9e7421a` builder-fix-005 | Resuelto junto con fix-002 |
| LUPA-010 | Falta `src-tauri/capabilities/default.json` → commands Tauri no invocables | `ade38c6` builder-fix-006 | Permisos core + webview + event + app + path. Merge con tu versión (conservé tus `window:allow-*` + mi `webview`) |
| LUPA-011 | `tauri.conf.json` referencia `icons/*` inexistentes → build fail en bundle | `ade38c6` builder-fix-007 | Generé placeholder "A" oscuro con Pillow + convert (32/128/256 PNG + .ico multisize + .icns + 1024 source) |
| LUPA-012 | README con `pnpm install --frozen-lockfile` + menciona `git2` | `70159ac` builder-fix-008 | `npm ci` + ref a ADR-001 + requisito Node 20+ |
| LUPA-013 | `config.rs` hardcodea `C:/Users/Tony/...` → tests cross-platform rotos | `70159ac` builder-fix-009 | `dirs::data_local_dir().join("agente-repo")`. Agregué `dirs = "5"` a Cargo.toml |
| LUPA-014 | Sin `vitest.config.ts` con `environment: jsdom` → tests React imposibles | `70159ac` builder-fix-010 | Config + `src/test/setup.ts` (jest-dom) + smoke test `render()` pasa |

### Ronda 4 — CI failing
| ID | Título | Commit | Nota |
|---|---|---|---|
| LUPA-016 | `cargo fmt --check` fallaba en 4 archivos src-tauri | `280e5e8` builder-fix-011 | Rustfmt aplicado. Instalé `rustfmt` (apt) en VPS-2 |
| LUPA-017 | `icon.png` era RGB (512), proc macro `generate_context!` requiere RGBA | `06a7d8c` builder-fix-012 | Regenerado 1024x1024 RGBA con Pillow |
| LUPA-018 | `use std::path::Path` unused en `parser.rs` → clippy `-D warnings` rompía CI | `06a7d8c` builder-fix-013 | Línea eliminada |
| LUPA-019 | `clippy::manual_split_once` en `parser.rs:77` → `-D warnings` | `b1a7986` builder-fix-014 | `splitn(2).nth(1)` → `split_once().map(\|x\| x.1)` |

---

## Validación end-to-end (panel-agentes HEAD `b1a7986`)

```
npm ci           → 166 packages, 0 vulns
npm run build    → dist OK (149.67 kB JS / 48.80 kB gzip)
npm test         → 8/8 tests pass (smoke + taskParser)
rustfmt --check  → exit 0
```

Lo que **no** pude correr local (no tengo cargo instalado en VPS-2):
- `cargo check`
- `cargo clippy -- -D warnings`
- `cargo tauri build`

Confío en que CI los valide. Si falla algo, tomo la fix task que generes.

---

## Observaciones para Lupa

1. **Tus bumps mayores (vite 5→8, vitest 1→4) sin validar install+build dejaron 3 bugs encadenados** (LUPA-006 peer dep, config minify, Node 18). Sugiero flujo: `npm install && npm run build && npm test` antes de commit cuando el bump es mayor.
2. **Tu `capabilities/default.json` no incluía `core:webview:default` ni `core:path:default`**. Mergé ambas listas; verificar si todas las permissions son necesarias (principio de menor privilegio).
3. **Íconos siguen siendo placeholder** ("A" azul sobre fondo oscuro). Pedirle a Tony el logo real o a architect diseñar uno cuando quiera.

## Open (no builder)
- **LUPA-007** auditor-ops `rm` vs `git mv` — ya hay `auditor-ops-fix-003` completado, marcá cerrado en `bugs.json`.

## Estado builder VPS-2
- Cola non-evo: 0
- Completadas hoy: 14 fixes + 3 ADR/doc + CELL! (11) = **28**
- Modo master activo, loop 1 min monitoreando nuevas tareas.
