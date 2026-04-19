# Rust — thiserror vs anyhow

## TL;DR
- **`thiserror`** → libraries / APIs públicas: define enums `Error` tipados para que el caller pueda `match` por variante.
- **`anyhow`** → código de aplicación / bins: error opaco tipo `Box<dyn Error>` con backtrace, para el happy path.
- Regla: ¿el caller necesita reaccionar distinto según el fallo? → thiserror. ¿Solo lo loggea y sigue? → anyhow.

## Detalles

### thiserror (library layer)
```rust
use thiserror::Error;

#[derive(Debug, Error)]
pub enum AgentError {
    #[error("archivo no encontrado: {0}")]
    NotFound(PathBuf),
    #[error("parseo fallido: {0}")]
    Parse(#[from] serde_json::Error),
    #[error("IO: {0}")]
    Io(#[from] std::io::Error),
}
```
- `#[from]` auto-convierte errores con `?`.
- El caller puede: `match err { AgentError::NotFound(_) => retry, _ => bubble }`.

### anyhow (application layer)
```rust
use anyhow::{Context, Result};

fn read_state() -> Result<State> {
    let raw = fs::read_to_string("STATE.md")
        .context("leyendo STATE.md")?;
    parse(&raw).context("parseando STATE")
}
```
- `.context()` agrega capa de info sin definir tipo nuevo.
- Backtrace automático con `RUST_BACKTRACE=1`.

### Regla dura
- **Nunca** expongas `anyhow::Error` en API pública de crate → los callers pierden poder matchear.
- En un workspace Tauri: `src-tauri/src/lib.rs` (módulos internos) → thiserror. `src-tauri/src/main.rs` + comandos handlers → anyhow para top-level. Los comandos Tauri necesitan `Result<T, String>` o error tipado serializable.

### Serializable para Tauri commands
thiserror + `serde::Serialize`:
```rust
#[derive(Debug, Error, Serialize)]
#[serde(tag = "type", content = "message")]
pub enum TauriError { ... }
```

## Fuente
- https://oneuptime.com/blog/post/2026-01-25-error-types-thiserror-anyhow-rust/view
- https://dev.to/leapcell/rust-error-handling-compared-anyhow-vs-thiserror-vs-snafu-2003
- https://www.shakacode.com/blog/thiserror-anyhow-or-how-i-handle-errors-in-rust-apps/

## Aplicabilidad a panel-agentes
**Sí, mezclar ambos.** Plan:
- `src-tauri/src/errors.rs` con `AgentError` en thiserror (variantes: `NotFound`, `Parse`, `Io`, `GitFailed`, `InvalidState`). Serializable para cruzar IPC → frontend muestra mensaje tipado.
- `src-tauri/src/main.rs` + funciones helper → `anyhow::Result<T>` + `.context(...)` para debugging.
- `#[tauri::command]` devuelve `Result<T, AgentError>`. El frontend hace type-narrowing.
