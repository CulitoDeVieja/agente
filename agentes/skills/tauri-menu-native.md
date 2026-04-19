# tauri-menu-native

## Propósito
Crear el menú nativo de la aplicación (barra de menú) con Tauri 2 en macOS/Windows/Linux.

## Cuándo usar
- Apps de escritorio que siguen las convenciones de la plataforma
- Acceso a comandos vía teclado (Cmd+S, Ctrl+Z, etc.)
- macOS requiere menú nativo para funcionar correctamente

## Pasos

1. Construir el menú en `src-tauri/src/lib.rs`:
   ```rust
   use tauri::menu::{Menu, Submenu, MenuItem, PredefinedMenuItem};

   tauri::Builder::default()
       .setup(|app| {
           let file_menu = Submenu::with_items(app, "Archivo", true, &[
               &MenuItem::with_id(app, "new", "Nuevo", true, Some("CmdOrCtrl+N"))?,
               &MenuItem::with_id(app, "open", "Abrir", true, Some("CmdOrCtrl+O"))?,
               &PredefinedMenuItem::separator(app)?,
               &PredefinedMenuItem::quit(app, Some("Salir"))?,
           ])?;

           let edit_menu = Submenu::with_items(app, "Editar", true, &[
               &PredefinedMenuItem::undo(app, Some("Deshacer"))?,
               &PredefinedMenuItem::redo(app, Some("Rehacer"))?,
               &PredefinedMenuItem::copy(app, None)?,
               &PredefinedMenuItem::paste(app, None)?,
           ])?;

           let menu = Menu::with_items(app, &[&file_menu, &edit_menu])?;
           app.set_menu(menu)?;

           app.on_menu_event(|app, event| {
               match event.id().as_ref() {
                   "new" => { /* manejar nuevo */ }
                   "open" => { /* manejar abrir */ }
                   _ => {}
               }
           });
           Ok(())
       })
   ```

## Ejemplo
En macOS, el menú "Archivo → Nuevo" (Cmd+N) crea un nuevo documento.

## Errores comunes
- **Menú no aparece en macOS**: debe asignarse con `app.set_menu()` en el setup hook.
- **Shortcut no funciona**: el formato es `"CmdOrCtrl+K"` — no usar `Ctrl` directamente.
- **PredefinedMenuItem::quit no cierra**: asegurarse de no tener un listener `close-requested` que lo cancele.
