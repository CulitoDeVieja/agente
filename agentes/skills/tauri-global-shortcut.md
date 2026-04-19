# tauri-global-shortcut

## Propósito
Registrar atajos de teclado globales que funcionan aunque la app no tenga el foco, usando Tauri 2.

## Cuándo usar
- Traer la ventana al frente con un atajo (como apps de nota rápida)
- Capturar screenshots o iniciar grabaciones globalmente
- Activar funciones sin cambiar de app

## Pasos

1. Agregar el plugin en `src-tauri/Cargo.toml`:
   ```toml
   tauri-plugin-global-shortcut = "2"
   ```

2. Registrar en `src-tauri/src/lib.rs`:
   ```rust
   use tauri_plugin_global_shortcut::{Code, GlobalShortcutExt, Modifiers, Shortcut};

   tauri::Builder::default()
       .plugin(tauri_plugin_global_shortcut::Builder::new()
           .with_shortcut(Shortcut::new(
               Some(Modifiers::SUPER | Modifiers::SHIFT),
               Code::KeyK,
           ))?
           .with_handler(|app, shortcut, _event| {
               if let Some(win) = app.get_webview_window("main") {
                   let _ = win.show();
                   let _ = win.set_focus();
               }
           })
           .build()
       )?
   ```

3. También registrar/desregistrar dinámicamente desde JS:
   ```ts
   import { register, unregister } from '@tauri-apps/plugin-global-shortcut';

   await register('Super+Shift+K', () => {
     console.log('Atajo global activado');
   });

   // Al desmontar o en cleanup:
   await unregister('Super+Shift+K');
   ```

4. Permisos en capabilities:
   ```json
   { "permissions": ["global-shortcut:allow-register"] }
   ```

## Ejemplo
`Super+Shift+K` muestra la ventana oculta desde cualquier aplicación.

## Errores comunes
- **Conflicto con atajo del sistema**: elegir combinaciones poco comunes; algunas están reservadas por el OS.
- **No funciona en Wayland**: soporte limitado; fallback a X11 si es posible.
- **No se desregistra al cerrar**: agregar limpieza en el evento `window-destroyed` o al salir.
