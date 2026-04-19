# tauri-windows-build

## Propósito
Generar instalador `.msi` y ejecutable portable `.exe` para Windows con Tauri 2.

## Cuándo usar
Al preparar release de la app desktop para Windows.

## Pasos
1. Verificar `tauri.conf.json`:
   ```json
   "bundle": { "active": true, "targets": ["msi", "nsis"] }
   ```
2. Instalar WiX Toolset v3 (para `.msi`): descargarlo e incluirlo en PATH.
3. Build: `cargo tauri build`
4. Outputs en `src-tauri/target/release/bundle/`:
   - `msi/*.msi` — instalador
   - `nsis/*.exe` — portable/installer NSIS
5. Firmar (opcional): configurar `windows.certificateThumbprint` en `tauri.conf.json`.

## Ejemplo
```bash
cargo tauri build --target x86_64-pc-windows-msvc
# Output: src-tauri/target/x86_64-pc-windows-msvc/release/bundle/
```

## Errores comunes
- WiX no encontrado → agregar `C:\Program Files (x86)\WiX Toolset v3.x\bin` al PATH.
- Error de firma → omitir firma en dev con `"windows": { "certificateThumbprint": null }`.
- Build falla por icono faltante → `tauri.conf.json` requiere `icons/icon.ico` existente.
- Tamaño excesivo → activar `"strip": true` en perfil release de `Cargo.toml`.
