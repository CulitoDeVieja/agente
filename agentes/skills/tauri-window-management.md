# tauri-window-management

## Propósito
Crear, ocultar, mostrar, posicionar y controlar ventanas en Tauri 2.

## Cuándo usar
- Apps multi-ventana (panel principal + modal nativo)
- Ocultar ventana al minimizar en lugar de cerrar
- Centrar o posicionar ventanas programáticamente

## Pasos

1. Crear ventana adicional desde Rust:
   ```rust
   use tauri::{Manager, WebviewUrl, WebviewWindowBuilder};

   fn open_settings(app: &tauri::AppHandle) {
       WebviewWindowBuilder::new(app, "settings", WebviewUrl::App("settings".into()))
           .title("Configuración")
           .inner_size(600.0, 400.0)
           .resizable(false)
           .center()
           .build()
           .unwrap();
   }
   ```

2. Manipular ventana desde el frontend (JS):
   ```ts
   import { getCurrentWindow, Window } from '@tauri-apps/api/window';

   const win = getCurrentWindow();
   await win.hide();
   await win.show();
   await win.center();
   await win.setSize({ width: 800, height: 600 });
   await win.setPosition({ x: 100, y: 100 });
   ```

3. Prevenir cierre accidental (cerrar = ocultar):
   ```ts
   import { getCurrentWindow } from '@tauri-apps/api/window';

   getCurrentWindow().onCloseRequested(async (e) => {
     e.preventDefault();
     await getCurrentWindow().hide();
   });
   ```

4. Desde Rust, acceder a ventana por label:
   ```rust
   if let Some(win) = app.get_webview_window("main") {
       win.show().unwrap();
       win.set_focus().unwrap();
   }
   ```

## Ejemplo
Doble click en tray icon → mostrar ventana principal y traerla al frente.

## Errores comunes
- **Label duplicado**: cada ventana necesita un label único; lanzar la misma ventana dos veces falla.
- **`center()` no funciona en multi-monitor**: usar `set_position` con coordenadas del monitor deseado.
- **Ventana oculta al inicio**: configurar `visible: false` en `tauri.conf.json` y mostrar cuando esté lista.
