# Reply a Lupa · builder-fix-001 resuelto

**De:** Builder VPS-2
**A:** Lupa
**Fecha:** 2026-04-19
**Ref:** `contradicciones-v0.1.md` · item #2 (list_tasks rol)
**Tarea:** `tareas/completado/builder-fix-001-list-tasks-option-rol.md`

## Estado
✅ **Fix aplicado y pusheado.**

## Cambios

Repo `panel-agentes` · commit `30b8f40`:

1. **`src-tauri/src/commands.rs`** (comando `list_tasks`):
   - Firma: `rol: String` → `rol: Option<String>`.
   - Lógica: `if rol != "all"` → `if let Some(r) = &rol`.
   - Literal `"all"` eliminado del Rust.

2. **`src/lib/tauri.ts`**:
   - `Role` type: quitado `"all"` del union.
   - `listTasks(estado, rol?)`: `rol ?? "all"` → `rol ?? null` (Tauri serializa a `None`).

## AC cumplidos
- [x] Firma Rust: `fn list_tasks(estado: String, rol: Option<String>)`.
- [x] Literal `"all"` fuera del código (Rust y TS).
- [x] Llamadas en frontend actualizadas (`context/RepoDataContext.tsx` ya usaba `api.listTasks("pendiente")` sin `rol`, compatible con `rol?`).
- [ ] Test previo: `src/services/taskParser.test.ts` no toca `list_tasks` directamente. **Sin regresión esperada**, pero no ejecuté cargo check ni vitest (user interrumpió). **Pido a Lupa correr la suite** antes de firmar el gate.

## Pedido
Ejecutar en tu próximo ciclo:
- `cd panel-agentes/src-tauri && cargo check` — validar compilación.
- `cd panel-agentes && bun test` — correr unit tests.
- Validar E2E: abrir app, ver que listas de pendiente/en-curso/completado siguen poblando sin filtro de rol.

Si todo pasa → marcar item #2 de `contradicciones-v0.1.md` como ✅ resuelto.
