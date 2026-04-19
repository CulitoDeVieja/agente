# tauri-v2-breaking-changes

## Propósito
Referencia de los cambios breaking de Tauri v1 a v2 y cómo migrar el código.

## Cuándo usar
- Migrar un proyecto de Tauri v1 a v2
- Revisar por qué código v1 no compila en v2
- Onboarding a proyectos Tauri existentes

## Pasos

### Cambios principales

1. **Sistema de permisos** — el mayor cambio:
   - v1: `allowlist` en `tauri.conf.json`
   - v2: capabilities en `src-tauri/capabilities/*.json`
   ```json
   // v2: src-tauri/capabilities/default.json
   {
     "identifier": "default",
     "windows": ["main"],
     "permissions": ["core:default", "shell:allow-open", "fs:allow-read-dir"]
   }
   ```

2. **Plugins separados del core**:
   - v1: `tauri::api::shell`, `tauri::api::path` incluidos
   - v2: plugins separados (`tauri-plugin-shell`, `tauri-plugin-fs`, etc.)

3. **API de ventanas**:
   - v1: `import { appWindow } from '@tauri-apps/api/window'`
   - v2: `import { getCurrentWindow } from '@tauri-apps/api/window'`

4. **Invoke desde frontend**:
   - v1: `import { invoke } from '@tauri-apps/api/tauri'`
   - v2: `import { invoke } from '@tauri-apps/api/core'`

5. **Events**:
   - v1: `import { emit, listen } from '@tauri-apps/api/event'`
   - v2: igual, pero `app.emit()` en Rust requiere el trait `Emitter`

6. **Path API**:
   - v1: `import { appDir } from '@tauri-apps/api/path'`
   - v2: `import { appDataDir } from '@tauri-apps/api/path'` (renombrado)

7. **Migración automática**:
   ```bash
   npx @tauri-apps/migrate
   ```

### Checklist de migración
- [ ] Actualizar `@tauri-apps/api` a v2
- [ ] Migrar `allowlist` a capabilities
- [ ] Instalar plugins separados para shell, fs, dialog, etc.
- [ ] Actualizar imports de `@tauri-apps/api/tauri` → `@tauri-apps/api/core`
- [ ] Actualizar `appWindow` → `getCurrentWindow()`

## Ejemplo
Error común en migración: `invoke is not a function` → el import cambió de `@tauri-apps/api/tauri` a `@tauri-apps/api/core`.

## Errores comunes
- **Permisos denegados sin error claro**: en v2, falta de permisos retorna error genérico; revisar capabilities.
- **Plugin no encontrado**: los plugins v1 no son compatibles; buscar el equivalente v2 en `tauri-apps/plugins-workspace`.
- **`tauri::api::*` no existe**: todo fue movido a plugins separados en v2.
