# rust-panic-recovery

## Propósito
Capturar panics en Rust y recuperarse de ellos sin crashear la aplicación completa.

## Cuándo usar
- Comandos Tauri que ejecutan código potencialmente inestable
- Integración con librerías externas de las que no se confía
- Mantener la app corriendo ante errores inesperados en operaciones secundarias

## Pasos

1. Capturar un panic con `catch_unwind`:
   ```rust
   use std::panic;

   fn safe_parse(input: &str) -> Result<i32, String> {
       panic::catch_unwind(|| {
           input.parse::<i32>().unwrap() // este unwrap puede paniquear
       })
       .map_err(|e| {
           format!("Panic capturado: {:?}", e.downcast_ref::<&str>().unwrap_or(&"unknown"))
       })
   }
   ```

2. Usar en un comando Tauri:
   ```rust
   #[tauri::command]
   fn risky_operation(data: String) -> Result<String, String> {
       std::panic::catch_unwind(|| {
           process_data(&data) // función que podría paniquear
       })
       .map_err(|_| "Error interno inesperado".to_string())?
   }
   ```

3. Instalar un hook global de panic para logging:
   ```rust
   use std::panic;

   pub fn setup_panic_hook() {
       let original_hook = panic::take_hook();
       panic::set_hook(Box::new(move |info| {
           let location = info.location()
               .map(|l| format!("{}:{}", l.file(), l.line()))
               .unwrap_or_else(|| "ubicación desconocida".to_string());
           let message = info.payload()
               .downcast_ref::<&str>()
               .copied()
               .unwrap_or("panic sin mensaje");

           tracing::error!("PANIC en {}: {}", location, message);
           // Opcionalmente enviar a Sentry u otro servicio
           original_hook(info);
       }));
   }
   ```

4. Llamar en el setup de Tauri:
   ```rust
   fn main() {
       setup_panic_hook();
       tauri::Builder::default()
           .run(tauri::generate_context!())
           .expect("error al iniciar Tauri");
   }
   ```

5. Limitaciones de `catch_unwind`:
   - Solo funciona con panics que no abortan el proceso
   - No funciona si el compilador está configurado con `panic = "abort"` en Cargo.toml
   - No es un reemplazo de manejo de errores con `Result`

6. Verificar la estrategia de panic en `Cargo.toml`:
   ```toml
   [profile.release]
   panic = "unwind"  # necesario para catch_unwind
   # panic = "abort"  # más pequeño, pero catch_unwind no funciona
   ```

## Ejemplo
Comando que procesa archivos de terceros: `catch_unwind` previene que un archivo malformado crashee toda la app.

## Errores comunes
- **Confiar en `catch_unwind` como manejo de errores**: los panics son excepcionales; usar `Result` y `?` para errores esperados.
- **`panic = "abort"` en el perfil**: `catch_unwind` no funciona; cambiar a `"unwind"` si se necesita recuperación.
- **No loguear el panic**: capturar sin registrar el error hace imposible el debugging posterior.
