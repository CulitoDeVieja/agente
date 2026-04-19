# tauri-notifications

## Propósito
Enviar notificaciones nativas del sistema operativo desde una app Tauri 2.

## Cuándo usar
- Alertar al usuario cuando la app está en segundo plano
- Confirmar operaciones completadas fuera de la ventana activa
- Recordatorios o eventos programados

## Pasos

1. Agregar el plugin en `src-tauri/Cargo.toml`:
   ```toml
   tauri-plugin-notification = "2"
   ```

2. Registrar el plugin en `src-tauri/src/lib.rs`:
   ```rust
   tauri::Builder::default()
       .plugin(tauri_plugin_notification::init())
   ```

3. Configurar permisos en `src-tauri/capabilities/default.json`:
   ```json
   { "permissions": ["notification:default"] }
   ```

4. Instalar el wrapper JS:
   ```bash
   npm install @tauri-apps/plugin-notification
   ```

5. Usar desde el frontend:
   ```ts
   import { sendNotification, isPermissionGranted, requestPermission }
     from '@tauri-apps/plugin-notification';

   async function notify(title: string, body: string) {
     let granted = await isPermissionGranted();
     if (!granted) {
       const perm = await requestPermission();
       granted = perm === 'granted';
     }
     if (granted) sendNotification({ title, body });
   }
   ```

## Ejemplo
```ts
await notify('Exportación completa', 'Tu archivo fue guardado en /Descargas.');
```

## Errores comunes
- **No aparece la notificación**: verificar que el permiso `notification:default` esté en capabilities.
- **Windows silencia notificaciones**: el usuario puede tener el modo "No molestar" activo.
- **Sin icono**: en Windows, el icono proviene del ejecutable; asegurar que `tauri.conf.json` tenga un ícono válido.
