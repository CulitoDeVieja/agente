# tauri-shell-open

## Propósito
Abrir URLs en el browser predeterminado y archivos/carpetas en el explorador nativo desde Tauri 2.

## Cuándo usar
- Enlazar a documentación externa o sitios web
- Abrir la carpeta donde se guardó un archivo
- Lanzar archivos con su aplicación predeterminada

## Pasos

1. Agregar el plugin en `src-tauri/Cargo.toml`:
   ```toml
   tauri-plugin-shell = "2"
   ```

2. Registrar en `src-tauri/src/lib.rs`:
   ```rust
   tauri::Builder::default()
       .plugin(tauri_plugin_shell::init())
   ```

3. Configurar permisos en capabilities:
   ```json
   {
     "permissions": [
       "shell:allow-open"
     ]
   }
   ```

4. Instalar wrapper JS:
   ```bash
   npm install @tauri-apps/plugin-shell
   ```

5. Usar desde el frontend:
   ```ts
   import { open } from '@tauri-apps/plugin-shell';

   // Abrir URL en el browser
   await open('https://example.com');

   // Abrir carpeta en el explorador
   await open('/home/user/Documents');

   // Abrir archivo con app predeterminada
   await open('/home/user/report.pdf');
   ```

## Ejemplo
```ts
<button onClick={() => open('https://docs.myapp.com')}>
  Ver documentación
</button>
```

## Errores comunes
- **Permiso denegado**: `shell:allow-open` debe estar explícito en capabilities; no está por defecto.
- **Ruta con espacios falla en Windows**: pasar la ruta tal cual — el plugin maneja el escape.
- **No abre en Linux sin DE**: en entornos sin escritorio gráfico, `xdg-open` puede no estar disponible.
