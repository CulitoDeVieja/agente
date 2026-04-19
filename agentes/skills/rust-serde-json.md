# rust-serde-json

## Propósito
Serializar y deserializar JSON en Rust con serde para usarlo en commands Tauri.

## Cuándo usar
Al definir structs que se envían/reciben entre Rust y el frontend React.

## Pasos
1. En `Cargo.toml`:
   ```toml
   serde = { version = "1", features = ["derive"] }
   serde_json = "1"
   ```
2. Definir struct:
   ```rust
   #[derive(serde::Serialize, serde::Deserialize)]
   pub struct Task {
       pub id: String,
       pub rol: String,
       pub estado: String,
   }
   ```
3. Retornar desde command: `Ok(task)` — Tauri serializa automáticamente.
4. Parsear JSON manual: `let task: Task = serde_json::from_str(&json_str)?;`

## Ejemplo
```rust
#[tauri::command]
pub fn get_tasks(dir: String) -> Result<Vec<Task>, String> {
    // leer archivos y parsear → Vec<Task>
}
```

## Errores comunes
- Campo faltante en JSON → usar `#[serde(default)]` en campos opcionales.
- Snake_case Rust vs camelCase JS → agregar `#[serde(rename_all = "camelCase")]` al struct.
- Error de compilación "trait not implemented" → verificar que ambos `Serialize` y `Deserialize` están en el derive.
