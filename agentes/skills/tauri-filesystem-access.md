# tauri-filesystem-access

## Propósito
Leer y listar archivos del sistema desde Rust en Tauri 2 con permisos correctos.

## Cuándo usar
Al necesitar acceso a archivos locales desde la app Tauri.

## Pasos
1. En `src-tauri/capabilities/default.json` agregar permisos:
   ```json
   "permissions": ["fs:read-all", "fs:read-dirs"]
   ```
2. Command para listar directorio:
   ```rust
   #[tauri::command]
   pub fn list_dir(path: String) -> Result<Vec<String>, String> {
       std::fs::read_dir(&path)
           .map_err(|e| e.to_string())?
           .filter_map(|e| e.ok())
           .map(|e| e.file_name().to_string_lossy().into_owned())
           .collect::<Vec<_>>()
           .pipe(Ok)
   }
   ```
3. Para leer archivo: `std::fs::read_to_string(path).map_err(|e| e.to_string())`

## Ejemplo
```rust
#[tauri::command]
pub fn read_task(path: String) -> Result<String, String> {
    std::fs::read_to_string(&path).map_err(|e| e.to_string())
}
```

## Errores comunes
- Permisos no declarados → operación falla en producción, pasa en dev.
- Paths Windows vs Unix → usar `std::path::Path` en lugar de strings crudos.
- Directorio no existe → manejar `ErrorKind::NotFound` explícitamente.
