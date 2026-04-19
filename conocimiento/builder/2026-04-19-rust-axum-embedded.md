# Axum embebido en Tauri

## TL;DR
- Podés correr un **servidor Axum dentro del proceso Tauri** (mismo binary) o como **sidecar** (proceso separado).
- Casos de uso: server HTTP local para HTMX, WebSocket, exponer API a apps externas, o proxy de dev.
- Para IPC entre frontend y Rust, `invoke()` nativo de Tauri es más simple — Axum solo si necesitás protocolo HTTP real.

## Detalles

### Opción 1: Axum in-process con tauri::async_runtime
```rust
use axum::{Router, routing::get};
use tauri::Manager;

fn main() {
    tauri::Builder::default()
        .setup(|app| {
            tauri::async_runtime::spawn(async {
                let app = Router::new().route("/ws", get(ws_handler));
                let listener = tokio::net::TcpListener::bind("127.0.0.1:0").await.unwrap();
                let port = listener.local_addr().unwrap().port();
                // Emit port to frontend
                axum::serve(listener, app).await.unwrap();
            });
            Ok(())
        })
        .run(tauri::generate_context!()).unwrap();
}
```
Puerto dinámico + loopback = zero exposición a red.

### Opción 2: Sidecar binary
```json
// tauri.conf.json
"bundle": {
  "externalBin": ["binaries/agent-server"]
}
```
Binario Rust separado con su propio Axum, lanzado desde Tauri con `Command::new_sidecar()`. Útil si querés aislar o reusar el server fuera de Tauri.

### Plugin oficial `tauri-plugin-axum`
Embebe Axum en el protocolo custom de Tauri (no TCP real). Rutas accesibles vía `fetch('http://tauri.localhost/...')` desde webview. Ventaja: cero puerto abierto.

### Cuándo **no** usar Axum
- Comunicación simple frontend ↔ Rust → `#[tauri::command]` es más simple y tipado (ver evo 108).
- CRUD local → SQLite + commands tipados.

### Cuándo **sí**
- Streaming de logs en tiempo real (SSE/WebSocket).
- Expone API a herramientas externas (CLI, VSCode extension).
- HTMX-style UI server-rendered.

## Fuente
- https://crates.io/crates/tauri-axum-htmx
- https://docs.rs/tauri-plugin-axum
- https://evilmartians.com/chronicles/making-desktop-apps-with-revved-up-potential-rust-tauri-sidecar

## Aplicabilidad a panel-agentes
**Opcional v0.2.** v0.1 no necesita Axum — `invoke()` cubre todo (listar agentes, leer STATE, ejecutar git). **Futuro:** si Tony quiere monitorear desde navegador móvil (en lugar de Tauri mobile), Axum on-port con WebSocket para logs stream. Alternativa más simple: `tauri-plugin-axum` sobre protocolo custom — misma DX sin puerto abierto.
