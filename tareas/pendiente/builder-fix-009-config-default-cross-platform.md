# Fix: `repo_path` default Windows-only

**Rol:** builder
**Severidad:** media
**Detectado por:** auditor-ops (subagente audit dinámico) → bug LUPA-013

## Problema
`src-tauri/src/config.rs:14` hardcodea:
```rust
"C:/Users/Tony/AppData/Local/Temp/agente-repo"
```
Rompe en CI Linux/macOS y dificulta tests cross-platform.

## Fix
Usar `dirs::data_local_dir()` o `tauri::api::path::app_local_data_dir(&config)`:
```rust
fn default_repo_path() -> PathBuf {
    dirs::data_local_dir()
        .unwrap_or_else(|| PathBuf::from("."))
        .join("agente-repo")
}
```

## AC
- [ ] Default cross-platform.
- [ ] Tests pasan en Linux + Windows.

## Depende de: (ninguna)
