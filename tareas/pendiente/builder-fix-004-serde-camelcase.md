# Fix: Serde structs deben rename_all = "camelCase"

**Rol:** builder
**Severidad:** ALTA
**Detectado por:** auditor-ops (subagente audit dinámico) → bug LUPA-008
**Fecha:** 2026-04-19

## Problema
Backend Rust serializa `snake_case` (`depende_de`, `created_at`, `ultima_senal`, `modo_master_active`, `repo_path`, `refresh_timeout_ms`, `auto_refresh_on_focus`).
Frontend TS espera `camelCase` (`dependeDe`, `createdAt`, `ultimaSenal`, `modoMasterActive`, `repoPath`, etc.).
**Resultado:** `state.modoMasterActive` siempre `undefined`. Panel no muestra MODO MASTER, contadores rotos. Falla silenciosa en runtime.

## Fix
Agregar `#[serde(rename_all = "camelCase")]` a:
- `src-tauri/src/parser.rs` → struct `Task`
- `src-tauri/src/parser.rs` → struct `AgentSnapshot`
- `src-tauri/src/parser.rs` → struct `StateSnapshot`
- `src-tauri/src/config.rs` → struct `AppConfig`

## AC
- [ ] 4 structs con `#[serde(rename_all = "camelCase")]`.
- [ ] Test E2E manual: panel muestra MODO MASTER cuando `state.json` lo indica.
- [ ] `npm test` sigue 7/7.

## Depende de: (ninguna)
