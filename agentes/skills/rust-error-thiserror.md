# rust-error-thiserror

## Propósito
Definir tipos de error idiomáticos en Rust con el crate `thiserror` para manejo de errores expresivo.

## Cuándo usar
- Librerías o módulos Rust que necesitan tipos de error propios
- Comandos Tauri que retornan errores tipados al frontend
- Reemplazar strings como errores (`Err("algo falló")`) por tipos estructurados

## Pasos

1. Agregar en `Cargo.toml`:
   ```toml
   thiserror = "1"
   ```

2. Definir el tipo de error:
   ```rust
   use thiserror::Error;

   #[derive(Debug, Error)]
   pub enum AppError {
       #[error("No se encontró el archivo: {path}")]
       FileNotFound { path: String },

       #[error("Error de base de datos: {0}")]
       Database(#[from] rusqlite::Error),

       #[error("Error de red: {0}")]
       Network(#[from] reqwest::Error),

       #[error("Configuración inválida: {message}")]
       InvalidConfig { message: String },
   }
   ```

3. Usar en funciones:
   ```rust
   fn load_file(path: &str) -> Result<String, AppError> {
       std::fs::read_to_string(path).map_err(|_| AppError::FileNotFound {
           path: path.to_string(),
       })
   }
   ```

4. El operador `?` con `#[from]` hace conversión automática:
   ```rust
   async fn get_data(url: &str) -> Result<String, AppError> {
       let resp = reqwest::get(url).await?;  // reqwest::Error → AppError::Network
       Ok(resp.text().await?)
   }
   ```

5. Serializar para Tauri (el frontend recibe el mensaje de error):
   ```rust
   impl serde::Serialize for AppError {
       fn serialize<S: serde::Serializer>(&self, s: S) -> Result<S::Ok, S::Error> {
           s.serialize_str(&self.to_string())
       }
   }

   #[tauri::command]
   async fn risky_command() -> Result<String, AppError> {
       load_file("/etc/app.conf")
   }
   ```

## Ejemplo
`AppError::Database(e)` muestra "Error de base de datos: UNIQUE constraint failed" en el frontend.

## Errores comunes
- **`#[from]` en tipos que no implementan `Error`**: solo funciona con tipos que implementan `std::error::Error`.
- **No serializar el error para Tauri**: el frontend recibe un objeto opaco si `AppError` no es serializable.
- **Usar `anyhow` en lugar de `thiserror` para libs**: `thiserror` es para definir errores; `anyhow` es para propagarlos.
