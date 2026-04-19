# tauri-system-tray

## Propósito
Agregar un ícono en la bandeja del sistema (system tray) con menú contextual usando Tauri 2.

## Cuándo usar
- Apps que corren en segundo plano y necesitan acceso rápido
- Ocultar la ventana principal sin cerrar la app
- Acciones rápidas desde el tray (abrir, salir, estado)

## Pasos

1. Habilitar el plugin en `src-tauri/Cargo.toml`:
   ```toml
   tauri-plugin-tray = "2"
   ```

2. Configurar en `src-tauri/src/lib.rs`:
   ```rust
   use tauri::{
       menu::{Menu, MenuItem},
       tray::{MouseButton, TrayIconBuilder, TrayIconEvent},
       Manager,
   };

   tauri::Builder::default()
       .setup(|app| {
           let quit = MenuItem::with_id(app, "quit", "Salir", true, None::<&str>)?;
           let show = MenuItem::with_id(app, "show", "Mostrar", true, None::<&str>)?;
           let menu = Menu::with_items(app, &[&show, &quit])?;

           TrayIconBuilder::new()
               .icon(app.default_window_icon().unwrap().clone())
               .menu(&menu)
               .on_menu_event(|app, event| match event.id.as_ref() {
                   "quit" => app.exit(0),
                   "show" => {
                       if let Some(w) = app.get_webview_window("main") {
                           let _ = w.show();
                       }
                   }
                   _ => {}
               })
               .build(app)?;
           Ok(())
       })
   ```

3. Agregar ícono de tray en `tauri.conf.json`:
   ```json
   { "bundle": { "icon": ["icons/icon.png"] } }
   ```

## Ejemplo
Click derecho en el tray → "Mostrar" devuelve la ventana principal al frente.

## Errores comunes
- **Ícono no aparece en Linux**: necesita `libayatana-appindicator` instalado en el sistema.
- **Menú no se cierra**: llamar `menu.set_as_app_menu` solo en macOS; en Windows/Linux es contextual.
- **App no cierra**: si `prevent_default_window_close` está activo, manejar el evento `close-requested`.
