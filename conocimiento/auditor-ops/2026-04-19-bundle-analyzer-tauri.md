# Bundle analyzer para Tauri

## TL;DR
- Tauri ≠ una sola pieza: medir **frontend (rollup)** y **binario Rust (cargo-bloat)** por separado.
- Frontend: `rollup-plugin-visualizer`. Backend: `cargo-bloat`.
- Tauri base ≈ 2-10 MB total vs Electron 80-200 MB.

## Detalles

**Frontend (Vite + React):**
```ts
// vite.config.ts
import { visualizer } from 'rollup-plugin-visualizer';
export default { plugins: [visualizer({ filename: 'stats.html', open: true })] }
```
Genera HTML interactivo con treemap de bundle.

**Backend (Rust src-tauri):**
```bash
cargo install cargo-bloat
cargo bloat --release --crates -n 20  # top 20 crates por tamaño
cargo bloat --release -n 30            # top 30 funciones
```

**Optimización en `src-tauri/Cargo.toml`:**
```toml
[profile.release]
opt-level = "s"
lto = true
codegen-units = 1
strip = true
panic = "abort"
```

## Fuente
- https://v2.tauri.app/concept/size/
- https://github.com/tauri-apps/tauri (cargo-bloat referenced en docs)
- https://www.pkgpulse.com/blog/electron-vs-tauri-2026

## Aplicabilidad
**Sí — directo.** Tareas auditor-ops-015 + auditor-ops-032 usan estos tools. Agregar cargo-bloat al README de panel-agentes y al CI para alertar regresiones >10% en tamaño binario.
