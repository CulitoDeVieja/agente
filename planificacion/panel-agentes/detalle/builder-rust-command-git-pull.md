# Plan: rust-command-git-pull

## Objetivo
Implementar comando `git_pull()` que ejecuta `git pull --rebase` en el repo local.

## Signature
```rust
#[tauri::command]
fn git_pull() -> GitResult
```

## Pasos
1. Obtener path del repo desde config (variable de entorno o tauri.conf)
2. Ejecutar `std::process::Command::new("git").args(["pull", "--rebase"])`
3. Capturar stdout/stderr
4. Retornar `GitResult { ok: bool, message: String }`

## Tests planeados
- [ ] Retorna `ok: true` en repo limpio
- [ ] Retorna `ok: false` con mensaje si hay conflicto
- [ ] Path configurable via env var `AGENTE_REPO_PATH`
