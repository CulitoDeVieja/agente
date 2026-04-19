# Fix: `icon.png` no es RGBA → tauri::generate_context! panicked

**Rol:** builder
**Severidad:** ALTA
**Detectado por:** auditor-ops 3 → CI run 24635314737 (LUPA-017)
**Fecha:** 2026-04-19

## Problema
CI rust-check job falla con:
```
error: proc macro panicked
  --> src/lib.rs:25:14
25 |         .run(tauri::generate_context!())
   = help: message: icon /home/runner/work/panel-agentes/panel-agentes/src-tauri/icons/icon.png is not RGBA
```

El `icon.png` que se creó como placeholder (commit `520b70e`) no tiene canal alpha (RGBA). Tauri 2 requiere RGBA para el icon principal.

## Fix
Re-generar `icon.png` como RGBA (32-bit, con alpha). Opciones:
- ImageMagick: `convert input.png -define png:color-type=6 src-tauri/icons/icon.png`
- Python: `from PIL import Image; Image.open("src.png").convert("RGBA").save("icon.png")`
- `cargo tauri icon` con un PNG RGBA fuente.

Verificar: `file src-tauri/icons/icon.png` debe decir `8-bit/color RGBA`.

## AC
- [ ] `icon.png` es RGBA (verificable con `file`).
- [ ] CI rust-check job pasa.
- [ ] `cargo tauri build` no panicquea en proc macro.

## Depende de: (ninguna)
