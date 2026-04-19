# websockets-rust-tauri

## Propósito
Establecer conexiones WebSocket en Rust e integrarlas con el frontend de Tauri 2 via eventos.

## Cuándo usar
- Recibir actualizaciones en tiempo real desde un servidor (chat, precios, estado)
- Comunicación bidireccional con un servidor externo
- Streaming de datos desde el backend Rust al frontend React

## Pasos

1. Agregar dependencias en `Cargo.toml`:
   ```toml
   tokio = { version = "1", features = ["full"] }
   tokio-tungstenite = "0.24"
   futures-util = "0.3"
   ```

2. Conectar y escuchar mensajes en Rust:
   ```rust
   use tokio_tungstenite::connect_async;
   use futures_util::StreamExt;
   use tauri::{AppHandle, Emitter};

   #[tauri::command]
   pub async fn start_ws(app: AppHandle, url: String) {
       let (ws_stream, _) = connect_async(&url).await.expect("No se pudo conectar");
       let (_, mut read) = ws_stream.split();

       tokio::spawn(async move {
           while let Some(msg) = read.next().await {
               if let Ok(msg) = msg {
                   if msg.is_text() {
                       app.emit("ws-message", msg.to_text().unwrap()).unwrap();
                   }
               }
           }
       });
   }
   ```

3. Registrar el comando en `lib.rs`:
   ```rust
   tauri::Builder::default()
       .invoke_handler(tauri::generate_handler![start_ws])
   ```

4. Escuchar en el frontend React:
   ```ts
   import { invoke } from '@tauri-apps/api/core';
   import { listen } from '@tauri-apps/api/event';

   await invoke('start_ws', { url: 'ws://localhost:3001' });

   const unlisten = await listen<string>('ws-message', (event) => {
     const data = JSON.parse(event.payload);
     setMessages(prev => [...prev, data]);
   });

   // Cleanup
   return () => { unlisten(); };
   ```

## Ejemplo
Dashboard de precios en tiempo real: Rust conecta al feed WS y emite eventos por cada tick.

## Errores comunes
- **No usar `tokio::spawn`**: bloquear el hilo principal impide que Tauri responda.
- **No hacer cleanup del listener**: llamar siempre `unlisten()` al desmontar el componente.
- **TLS en `wss://`**: agregar feature `native-tls` en tokio-tungstenite para conexiones seguras.
