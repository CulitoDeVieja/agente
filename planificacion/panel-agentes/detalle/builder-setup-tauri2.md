# Plan: setup-tauri2

## Objetivo
Configurar Tauri 2 en el repo panel-agentes.

## Inputs
- Repo con Vite+React+TS funcionando (builder-011)

## Pasos
1. Verificar `src-tauri/Cargo.toml` tiene `tauri = { version = "2", features = ["shell-open"] }`
2. Configurar `src-tauri/tauri.conf.json`:
   - `productName`: "panel-agentes"
   - `identifier`: "com.agentes.panel"
   - `windows`: title, width 1200, height 800
   - `security.csp`: null (dev)
3. Verificar `src-tauri/src/main.rs` invoca `tauri::Builder`
4. `cargo check` en `src-tauri/` sin errores

## Outputs
- `tauri.conf.json` configurado
- `Cargo.toml` con dependencias correctas
- `cargo check` pasa

## Tests planeados
- [ ] `cargo check` sin errores
- [ ] `npm run tauri dev` levanta webview
