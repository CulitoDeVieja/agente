# tauri-permissions-config

## Propósito
Configurar permisos granulares en Tauri 2 usando el sistema de capabilities para controlar el acceso a APIs del sistema.

## Cuándo usar
- Configurar qué APIs del sistema puede usar la app (fs, shell, dialog, etc.)
- Limitar el acceso por ventana o por origin
- Cumplir el principio de mínimo privilegio en apps desktop

## Pasos

1. Estructura del sistema de permisos en Tauri 2:
   ```
   src-tauri/
   ├── capabilities/
   │   ├── default.json      # permisos por defecto para todas las ventanas
   │   └── admin.json        # permisos extra para ventanas privilegiadas
   ```

2. Archivo de capability base:
   ```json
   // src-tauri/capabilities/default.json
   {
     "$schema": "../gen/schemas/desktop-schema.json",
     "identifier": "default",
     "description": "Permisos de la app principal",
     "windows": ["main"],
     "permissions": [
       "core:default",
       "shell:allow-open",
       "notification:default",
       "dialog:allow-open",
       "dialog:allow-save",
       "fs:allow-read-dir",
       "fs:allow-read-file",
       "fs:allow-write-file",
       "path:default"
     ]
   }
   ```

3. Permisos disponibles por plugin:

   | Plugin         | Permisos comunes                                          |
   |----------------|-----------------------------------------------------------|
   | core           | `core:default`, `core:app:allow-name`                    |
   | fs             | `fs:allow-read-file`, `fs:allow-write-file`, `fs:allow-read-dir` |
   | shell          | `shell:allow-open`, `shell:allow-execute`                |
   | dialog         | `dialog:allow-open`, `dialog:allow-save`, `dialog:allow-message` |
   | notification   | `notification:default`                                   |
   | global-shortcut| `global-shortcut:allow-register`                        |
   | autostart      | `autostart:default`                                      |

4. Restringir acceso a directorios específicos (scope):
   ```json
   {
     "permissions": [
       {
         "identifier": "fs:allow-read-file",
         "allow": [{ "path": "$APPDATA/*" }]
       },
       {
         "identifier": "fs:allow-write-file",
         "allow": [{ "path": "$APPDATA/*" }]
       }
     ]
   }
   ```

5. Capability por ventana específica:
   ```json
   {
     "identifier": "settings-window",
     "windows": ["settings"],
     "permissions": ["core:default"]
   }
   ```

## Ejemplo
La ventana principal tiene acceso a `fs` solo en `$APPDATA`; una ventana de preview solo tiene `core:default`.

## Errores comunes
- **Error "permission denied" sin mensaje claro**: habilitar logs en Rust para ver qué permiso falta: `RUST_LOG=tauri=debug`.
- **`fs:scope` no definido**: sin scope, algunos permisos de fs requieren paths explícitos.
- **Usar `core:all` en lugar de permisos granulares**: otorga demasiado acceso; preferir solo lo necesario.
