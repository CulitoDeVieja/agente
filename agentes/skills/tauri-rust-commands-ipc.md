# tauri-rust-commands-ipc

## Propósito
Exponer funciones Rust al frontend React via IPC de Tauri 2.

## Cuándo usar
Al agregar una nueva operación nativa (filesystem, procesos, red) accesible desde React.

## Pasos
1. Definir command en `src-tauri/src/commands/`:
   ```rust
   #[tauri::command]
   pub async fn read_file(path: String) -> Result<String, String> {
       std::fs::read_to_string(&path).map_err(|e| e.to_string())
   }
   ```
2. Registrar en `main.rs`: `.invoke_handler(tauri::generate_handler![read_file])`
3. Agregar permiso en `capabilities/default.json` si aplica filesystem.
4. Llamar desde React: `invoke<string>("read_file", { path })`
5. Manejar error: `invoke(...).catch(err => setError(err))`

## Ejemplo
```ts
import { invoke } from "@tauri-apps/api/core";
const content = await invoke<string>("read_file", { path: "/opt/agente/STATE.md" });
```

## Errores comunes
- Nombre del command en snake_case en Rust, camelCase en JS → Tauri convierte automáticamente.
- Parámetros no coinciden → error en runtime, no en compilación; verificar tipos.
- Sin permiso `fs:read-all` → operación bloqueada silenciosamente en producción.
