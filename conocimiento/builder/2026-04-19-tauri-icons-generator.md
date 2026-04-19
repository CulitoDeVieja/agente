# Tauri 2 — Generador de íconos multi-plataforma

## TL;DR
- Tauri CLI trae subcomando **`tauri icon`**: toma 1 PNG/SVG fuente y genera todos los íconos (Windows `.ico`, macOS `.icns`, Linux PNGs, iOS, Android).
- Input default: `./app-icon.png` (1024x1024, RGBA, 32-bit).
- Output: `src-tauri/icons/` con tamaños 32/128/256/512 + plataforma-específicos.

## Detalles

### Comando
```bash
bunx tauri icon ./assets/logo.png
# o con opciones
bunx tauri icon ./assets/logo.svg --ios-color "#0a0a0a" --output src-tauri/icons
```

### Requisitos del PNG fuente
- Cuadrado (width == height), ideal **1024x1024**.
- RGBA (4 canales).
- 32 bit por pixel (8 bit por canal).
- Con transparencia para macOS; sin transparencia para ios-color si querés fondo sólido iOS.

### Qué genera
| Plataforma | Archivos |
|---|---|
| Windows | `icon.ico` multi-size |
| macOS | `icon.icns` |
| Linux | `32x32.png`, `128x128.png`, `128x128@2x.png`, `icon.png` |
| iOS | set completo de AppIcons |
| Android | `mipmap-*` en todas las densidades |

### Config en `tauri.conf.json`
```json
"bundle": {
  "icon": [
    "icons/32x32.png",
    "icons/128x128.png",
    "icons/icon.icns",
    "icons/icon.ico"
  ]
}
```

### Regenerar en CI
Meter en `package.json`:
```json
"scripts": { "icons": "tauri icon ./assets/logo.png" }
```
No commitear `src-tauri/icons/` generados si el pipeline los regenera — evita drift.

## Fuente
- https://v2.tauri.app/develop/icons/
- https://v2.tauri.app/reference/cli/

## Aplicabilidad a panel-agentes
**Sí, 1 sola vez.** Workflow: Tony entrega logo 1024×1024 PNG → correr `bunx tauri icon` → commitear resultado en `src-tauri/icons/` (es lo que Tauri espera para bundling). Agregar script `bun run icons` para regeneración. No usar gen-iOS/Android (panel es Windows-only por ahora). Si más adelante el release v0.2 pide Mac, el comando ya deja el `.icns` listo.
