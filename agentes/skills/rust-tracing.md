# rust-tracing

## Propósito
Implementar logging estructurado y trazabilidad en aplicaciones Rust con el crate `tracing`.

## Cuándo usar
- Logging en el backend Rust de Tauri con niveles y campos estructurados
- Trazar el flujo de ejecución de funciones async
- Correlacionar logs de múltiples operaciones concurrentes

## Pasos

1. Agregar en `Cargo.toml`:
   ```toml
   tracing = "0.1"
   tracing-subscriber = { version = "0.3", features = ["env-filter"] }
   ```

2. Inicializar el subscriber en `main` o `lib.rs`:
   ```rust
   use tracing_subscriber::{layer::SubscriberExt, util::SubscriberInitExt, EnvFilter};

   tracing_subscriber::registry()
       .with(EnvFilter::try_from_default_env()
           .unwrap_or_else(|_| "info,mi_app=debug".into()))
       .with(tracing_subscriber::fmt::layer())
       .init();
   ```

3. Usar macros de tracing:
   ```rust
   use tracing::{debug, info, warn, error, instrument};

   info!("Aplicación iniciada");
   debug!(user_id = %user.id, "Cargando tareas");
   warn!(count = tasks.len(), "Muchas tareas pendientes");
   error!(err = ?e, path = %file_path, "No se pudo leer el archivo");
   ```

4. Instrumentar funciones async:
   ```rust
   #[instrument(skip(db), fields(user_id = %user_id))]
   async fn get_user_tasks(db: &Db, user_id: &str) -> Result<Vec<Task>, AppError> {
       debug!("Consultando tareas");
       db.query_tasks(user_id).await
   }
   ```
   El atributo `#[instrument]` crea un span automático para la función.

5. Filtrar por nivel con variable de entorno:
   ```bash
   RUST_LOG=debug cargo run
   RUST_LOG=mi_app=trace,tokio=warn cargo run
   ```

6. Para Tauri, inicializar en el setup hook:
   ```rust
   tauri::Builder::default()
       .setup(|_app| {
           tracing_subscriber::fmt().with_env_filter("info").init();
           Ok(())
       })
   ```

## Ejemplo
`#[instrument]` en un comando Tauri async registra automáticamente inicio, fin y duración del comando.

## Errores comunes
- **`init()` llamado dos veces**: panic; inicializar solo una vez, en el punto de entrada.
- **Campos con `%` vs `?`**: `%` usa `Display`, `?` usa `Debug`; para errores usar `?`, para strings `%`.
- **Sin EnvFilter**: sin filtro, en debug se genera demasiado ruido de tokio y otras deps.
