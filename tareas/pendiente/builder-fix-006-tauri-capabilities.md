# Fix: Falta `src-tauri/capabilities/` (Tauri 2 requirement)

**Rol:** builder
**Severidad:** ALTA
**Detectado por:** auditor-ops (subagente audit dinámico) → bug LUPA-010

## Problema
Tauri 2 requiere permisos declarados en `src-tauri/capabilities/*.json`. Sin esto, **ningún `#[tauri::command]` custom** (`list_tasks`, `read_state`, `git_pull`, `read_task`, etc.) es invocable desde el frontend. El panel arranca y queda en blanco.

## Fix
Crear `src-tauri/capabilities/default.json`:
```json
{
  "$schema": "../gen/schemas/desktop-schema.json",
  "identifier": "default",
  "description": "default capabilities",
  "windows": ["main"],
  "permissions": [
    "core:default",
    "core:window:default",
    "core:webview:default",
    "core:event:default"
  ]
}
```

Y agregar a `tauri.conf.json` allowlist con todos los commands custom (o usar tauri-plugin-shell + opener si aplica).

## AC
- [ ] `capabilities/default.json` existe.
- [ ] `cargo tauri dev` levanta + frontend puede invocar `list_tasks`.
- [ ] `npm test` sigue 7/7.

## Depende de: (ninguna)
