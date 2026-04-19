# tauri-auto-start

## Propósito
Registrar una app Tauri 2 para que inicie automáticamente al arrancar el sistema operativo.

## Cuándo usar
- Apps de productividad o monitoreo que deben correr desde el inicio
- Cuando el usuario activa "Iniciar con Windows/macOS" en las preferencias

## Pasos

1. Agregar el plugin en `src-tauri/Cargo.toml`:
   ```toml
   tauri-plugin-autostart = "2"
   ```

2. Registrar en `src-tauri/src/lib.rs`:
   ```rust
   use tauri_plugin_autostart::MacosLauncher;

   tauri::Builder::default()
       .plugin(tauri_plugin_autostart::init(
           MacosLauncher::LaunchAgent,
           Some(vec!["--minimized"]),
       ))
   ```

3. Instalar el wrapper JS:
   ```bash
   npm install @tauri-apps/plugin-autostart
   ```

4. Habilitar/deshabilitar desde el frontend:
   ```ts
   import { enable, disable, isEnabled }
     from '@tauri-apps/plugin-autostart';

   async function toggleAutostart(on: boolean) {
     if (on) await enable();
     else await disable();
   }

   const active = await isEnabled();
   ```

5. Agregar permiso en capabilities:
   ```json
   { "permissions": ["autostart:default"] }
   ```

## Ejemplo
```ts
// Al guardar preferencias del usuario
await toggleAutostart(settings.startWithSystem);
```

## Errores comunes
- **No funciona en Linux**: requiere un entorno de escritorio con autostart XDG (GNOME, KDE).
- **El argumento `--minimized` no se recibe**: parsear `std::env::args()` en Rust al inicio.
- **Se registra dos veces**: siempre llamar `isEnabled()` antes de `enable()`.
