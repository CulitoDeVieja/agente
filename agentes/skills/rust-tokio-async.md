# rust-tokio-async

## Propósito
Escribir código asíncrono en Rust con el runtime Tokio para operaciones concurrentes.

## Cuándo usar
- I/O async (HTTP, archivos, sockets) en el backend de Tauri
- Múltiples operaciones concurrentes sin bloquear el hilo
- Comandos Tauri que hacen fetching o trabajo en paralelo

## Pasos

1. Agregar en `Cargo.toml`:
   ```toml
   tokio = { version = "1", features = ["full"] }
   ```

2. Función async básica:
   ```rust
   use tokio::time::{sleep, Duration};

   async fn delayed_greeting(name: &str) -> String {
       sleep(Duration::from_millis(100)).await;
       format!("Hola, {name}!")
   }
   ```

3. Comando Tauri async:
   ```rust
   #[tauri::command]
   async fn fetch_data(url: String) -> Result<String, String> {
       let resp = reqwest::get(&url).await.map_err(|e| e.to_string())?;
       resp.text().await.map_err(|e| e.to_string())
   }
   ```

4. Concurrencia con `tokio::join!`:
   ```rust
   async fn fetch_all() {
       let (users, projects) = tokio::join!(
           fetch_users(),
           fetch_projects(),
       );
   }
   ```

5. Spawn de tareas en background:
   ```rust
   tokio::spawn(async move {
       loop {
           sync_data().await;
           tokio::time::sleep(Duration::from_secs(30)).await;
       }
   });
   ```

6. Channel para comunicar tareas:
   ```rust
   let (tx, mut rx) = tokio::sync::mpsc::channel::<String>(32);

   tokio::spawn(async move {
       while let Some(msg) = rx.recv().await {
           println!("Recibido: {msg}");
       }
   });

   tx.send("hola".to_string()).await.unwrap();
   ```

## Ejemplo
Dos llamadas HTTP en paralelo con `tokio::join!` en lugar de secuencial reduce el tiempo a la mitad.

## Errores comunes
- **`block_on` dentro de async**: llamar `block_on` dentro de una función async causa panic; usar `.await`.
- **No usar `tokio::spawn` para tareas largas**: bloquear el executor con operaciones síncronas pesadas.
- **Features de Tokio incompletas**: `features = ["full"]` en dev; en producción, especificar solo lo necesario.
