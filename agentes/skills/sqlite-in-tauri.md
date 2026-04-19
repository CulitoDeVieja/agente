# sqlite-in-tauri

## Propósito
Usar SQLite como base de datos local en una app Tauri 2 con el plugin oficial o rusqlite.

## Cuándo usar
- Almacenar datos estructurados localmente (tareas, proyectos, historial)
- Necesidad de queries SQL complejas
- Alternativa a JSON files cuando los datos crecen o necesitan índices

## Pasos

### Opción A: Plugin oficial tauri-plugin-sql

1. Agregar en `Cargo.toml`:
   ```toml
   tauri-plugin-sql = { version = "2", features = ["sqlite"] }
   ```

2. Registrar en `lib.rs`:
   ```rust
   tauri::Builder::default()
       .plugin(tauri_plugin_sql::Builder::default().build())
   ```

3. Usar desde JS:
   ```ts
   import Database from '@tauri-apps/plugin-sql';

   const db = await Database.load('sqlite:app.db');

   await db.execute(
     'CREATE TABLE IF NOT EXISTS tasks (id TEXT PRIMARY KEY, title TEXT, done INTEGER)'
   );

   await db.execute('INSERT INTO tasks VALUES ($1, $2, $3)', [id, title, 0]);

   const tasks = await db.select<Task[]>('SELECT * FROM tasks WHERE done = 0');
   ```

### Opción B: rusqlite desde comandos Rust

1. Agregar en `Cargo.toml`:
   ```toml
   rusqlite = { version = "0.31", features = ["bundled"] }
   ```

2. Comando Rust con rusqlite:
   ```rust
   use rusqlite::{Connection, Result};

   #[tauri::command]
   fn get_tasks(app: tauri::AppHandle) -> Result<Vec<String>, String> {
       let path = app.path().app_data_dir().unwrap().join("app.db");
       let conn = Connection::open(path).map_err(|e| e.to_string())?;
       let mut stmt = conn.prepare("SELECT title FROM tasks").map_err(|e| e.to_string())?;
       let titles: Vec<String> = stmt.query_map([], |r| r.get(0))
           .unwrap().filter_map(|r| r.ok()).collect();
       Ok(titles)
   }
   ```

## Ejemplo
Plugin SQL: ideal para queries desde el frontend. rusqlite: mejor para lógica de negocio compleja en Rust.

## Errores comunes
- **Path de la DB**: usar `app.path().app_data_dir()` — no rutas hardcodeadas.
- **Sin WAL mode**: habilitar `PRAGMA journal_mode=WAL` para concurrencia en lecturas.
- **Migraciones**: manejar versiones de schema con una tabla `migrations` o usar `sea-orm`.
