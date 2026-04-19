# Fix: `tauri.conf.json` referencia íconos inexistentes

**Rol:** builder
**Severidad:** ALTA
**Detectado por:** auditor-ops (subagente audit dinámico) → bug LUPA-011

## Problema
`src-tauri/tauri.conf.json:29-35` lista `icons/32x32.png`, `icons/128x128.png`, etc. **Pero `src-tauri/icons/` no existe.**
**Resultado:** `cargo tauri build` falla en el bundle stage.

## Fix
Opción A (rápido): generar íconos placeholder con `cargo tauri icon`:
```bash
cd src-tauri
cargo tauri icon ../icon-base.png   # requiere icon-base.png 1024x1024
```

Opción B (manual): crear `src-tauri/icons/` con los 4 PNG + .ico + .icns.

## AC
- [ ] `src-tauri/icons/` poblado (32x32, 128x128, 128x128@2x, icon.icns, icon.ico).
- [ ] `cargo tauri build` corre hasta bundle.
- [ ] `npm test` sigue 7/7.

## Depende de: (ninguna)
