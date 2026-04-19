# Tauri — window-vibrancy (blur/mica/acrylic)

## TL;DR
- Crate oficial **`window-vibrancy`** (tauri-apps) aplica efectos nativos: **Blur**, **Acrylic**, **Mica** (Win11), **Vibrancy** (macOS 10.10+).
- Linux no soporta: queda en manos del compositor.
- Integra en `setup()` de Tauri, necesita `transparent: true` en `tauri.conf.json`.

## Detalles

### Dependencia
```toml
# src-tauri/Cargo.toml
[dependencies]
window-vibrancy = "0.5"
```

### Setup en Tauri 2
```rust
use tauri::Manager;
use window_vibrancy::{apply_mica, apply_acrylic, apply_vibrancy, NSVisualEffectMaterial};

fn main() {
    tauri::Builder::default()
        .setup(|app| {
            let window = app.get_webview_window("main").unwrap();

            #[cfg(target_os = "windows")]
            apply_mica(&window, Some(true)) // true = dark
                .or_else(|_| apply_acrylic(&window, Some((18, 18, 18, 125))))
                .expect("no se pudo aplicar efecto");

            #[cfg(target_os = "macos")]
            apply_vibrancy(&window, NSVisualEffectMaterial::HudWindow, None, None)
                .expect("vibrancy falló");

            Ok(())
        })
        .run(tauri::generate_context!())
        .expect("error");
}
```

### Config de ventana
`tauri.conf.json`:
```json
"windows": [{
  "transparent": true,
  "decorations": true,
  "title": "Panel Agentes"
}]
```

### Caveats
- **Mica solo Win11** — fallback a Acrylic si falla (`or_else`).
- **Blur** en Win11 22621+ tiene lag al arrastrar → evitar para ventanas principales.
- Acrylic en Win10 v1903+ tiene drop de fps al resize.
- Frontend debe tener `body { background: transparent }` para que se vea.

## Fuente
- https://github.com/tauri-apps/window-vibrancy
- https://crates.io/crates/window-vibrancy
- https://dev.to/waradu/acrylic-window-effect-with-tauri-1078

## Aplicabilidad a panel-agentes
**Sí, opcional nice-to-have.** Tony usa Windows 11 (según plan) → Mica se ve premium y es nativo. Implementación: 10 líneas en `setup()` + `transparent: true`. Si falla en build de Tony, fallback silencioso a sólido (ya está el `.or_else`). **Posponer a post-v0.1:** primero funcionalidad, después coqueteo visual. Abrir issue `builder-post-v0.1-mica-effect` para tenerlo en radar.
