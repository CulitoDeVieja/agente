# json-file-storage

## Propósito
Persistir configuración y estado de la aplicación en archivos JSON desde Rust en Tauri 2.

## Cuándo usar
- Guardar preferencias del usuario (tema, idioma, ventana)
- Estado simple que no requiere SQL
- Configuración que el usuario puede editar manualmente

## Pasos

1. Agregar `serde` en `Cargo.toml`:
   ```toml
   serde = { version = "1", features = ["derive"] }
   serde_json = "1"
   ```

2. Definir la estructura de configuración:
   ```rust
   use serde::{Deserialize, Serialize};

   #[derive(Debug, Serialize, Deserialize, Default)]
   pub struct AppConfig {
       pub theme: String,
       pub language: String,
       pub window_width: u32,
       pub window_height: u32,
   }
   ```

3. Leer y escribir la config:
   ```rust
   use std::fs;
   use tauri::Manager;

   fn config_path(app: &tauri::AppHandle) -> std::path::PathBuf {
       app.path().app_config_dir().unwrap().join("config.json")
   }

   pub fn load_config(app: &tauri::AppHandle) -> AppConfig {
       let path = config_path(app);
       if path.exists() {
           let content = fs::read_to_string(&path).unwrap_or_default();
           serde_json::from_str(&content).unwrap_or_default()
       } else {
           AppConfig::default()
       }
   }

   pub fn save_config(app: &tauri::AppHandle, config: &AppConfig) {
       let path = config_path(app);
       if let Some(parent) = path.parent() {
           fs::create_dir_all(parent).ok();
       }
       let json = serde_json::to_string_pretty(config).unwrap();
       fs::write(path, json).ok();
   }
   ```

4. Exponer como comandos Tauri:
   ```rust
   #[tauri::command]
   fn get_config(app: tauri::AppHandle) -> AppConfig { load_config(&app) }

   #[tauri::command]
   fn set_config(app: tauri::AppHandle, config: AppConfig) { save_config(&app, &config); }
   ```

## Ejemplo
La config se guarda en `%APPDATA%\miapp\config.json` en Windows y `~/.config/miapp/config.json` en Linux.

## Errores comunes
- **Ruta hardcodeada**: usar siempre `app.path().app_config_dir()` para portabilidad cross-platform.
- **No crear directorio**: `create_dir_all` antes de escribir o el archivo falla si el dir no existe.
- **JSON corrupto**: usar `unwrap_or_default()` para no crashear con configs inválidas.
