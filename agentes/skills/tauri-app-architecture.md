# tauri-app-architecture

## Propósito
Estructura estándar de un proyecto Tauri 2 + React + TypeScript con IPC entre frontend y Rust.

## Cuándo usar
Al iniciar o auditar un proyecto desktop con Tauri 2.

## Pasos
1. Scaffold: `npm create tauri-app@latest -- --template react-ts`
2. Estructura de carpetas:
   ```
   src/           ← React + TS
   src-tauri/     ← Rust backend
     src/
       commands/  ← handlers #[tauri::command]
       main.rs
     tauri.conf.json
   ```
3. Definir commands Rust en `commands/`:
   ```rust
   #[tauri::command]
   pub fn list_tasks(dir: String) -> Result<Vec<String>, String> { ... }
   ```
4. Registrar en `main.rs`: `.invoke_handler(tauri::generate_handler![list_tasks])`
5. Invocar desde React: `await invoke<string[]>("list_tasks", { dir })`

## Ejemplo
```ts
import { invoke } from "@tauri-apps/api/core";
const tasks = await invoke<string[]>("list_tasks", { dir: "/opt/agente/tareas" });
```

## Errores comunes
- Command no registrado en `generate_handler!` → panic en runtime.
- Tipos Rust/TS desincronizados → usar `serde_json::Value` solo en prototipos, luego tipar.
- `tauri.conf.json` sin permisos `fs` → leer archivos falla silenciosamente.
- Hot reload no refleja cambios Rust → matar y reiniciar `cargo tauri dev`.
