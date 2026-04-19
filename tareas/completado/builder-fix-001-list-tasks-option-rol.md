# Fix: `list_tasks` — cambiar `rol: String` a `rol: Option<String>`

**Rol:** builder
**Severidad:** media
**Detectado por:** Lupa · contradicciones-v0.1.md #2
**Commit origen:** `edf93b1` (panel-agentes)

## Qué fixear
En `src-tauri/src/commands.rs`, comando `list_tasks`:
- Cambiar parámetro `rol: String` con valor mágico `"all"` → `rol: Option<String>`.
- Actualizar tipo TS en `src/types.ts`.
- Actualizar llamadas en frontend.

## AC
- [ ] Firma Rust: `fn list_tasks(estado: String, rol: Option<String>)`.
- [ ] Test que antes pasaba sigue pasando.
- [ ] No hay literal `"all"` en el código.

## Depende de: (ninguna)
