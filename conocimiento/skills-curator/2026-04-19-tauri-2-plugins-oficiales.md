# Tauri 2 — plugins oficiales

## TL;DR
- Tauri 2 modulariza features en plugins oficiales: fs, shell, dialog, notification, global-shortcut, updater, store, sql, etc.
- Se instalan con `npm install @tauri-apps/plugin-<nombre>` + crate Rust correspondiente.
- Configuración granular via `tauri.conf.json` + permisos en `capabilities/*.json`.

## Detalles

**Core:**
- `fs` — acceso a filesystem cross-platform.
- `shell` — spawn child processes (útil para `git`, scripts).
- `dialog` — file open/save + message dialogs nativos.
- `clipboard` — read/write del clipboard del sistema.

**UX:**
- `notification` — notificaciones nativas.
- `global-shortcut` — hotkeys fuera de foco.
- `http` — cliente HTTP Rust (alternativa a `fetch`).
- `updater` — in-app updates con firma criptográfica.

**Datos:**
- `store` — key-value persistente (config de app).
- `sql` — interfaz a bases SQL via sqlx (SQLite/Postgres/MySQL).
- `log` — logging configurable.

**Lifecycle:**
- `autostart` — arranque automático al bootear el SO.
- `single-instance` — previene múltiples instancias de la app.

**Instalación típica:**
```bash
npm install @tauri-apps/plugin-fs
# En src-tauri/Cargo.toml: tauri-plugin-fs = "2"
# En src-tauri/src/main.rs: .plugin(tauri_plugin_fs::init())
```

## Fuente
https://v2.tauri.app/plugin/

## Aplicabilidad a panel-agentes
**Sí — ya usamos varios.** Skills ya documentan: `fs` (filesystem-access), `shell` (run-cli-command), `notification` (notifications), `updater` (updater-future), `global-shortcut`, `auto-start`, `system-tray`.

Plugins **todavía no considerados** útiles para panel-agentes:
- `single-instance` — evitar múltiples paneles abiertos del mismo repo.
- `store` — persistir config de usuario (filtros, layout, tema) en lugar de JSON manual.
- `log` — reemplazar logging-strategy casero por este plugin oficial.
- `sql` — overkill ahora, pero útil si el panel empieza a cachear historia de tareas.

Próximo paso: considerar crear skills para `single-instance` y `store` (v0.2).
