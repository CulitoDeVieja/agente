# Decisión: Bundle size targets

**Decisión:** Target `.msi` final `<15MB` (definido en `00-resumen.md`). Sub-targets: JS bundle `<500KB` gzipped, Rust binary `<8MB` stripped.

## Alternativas
- **Sin target específico** — rechazado: sin número, nadie sabe si optimizar.
- **Target agresivo `<5MB`** — rechazado: Tauri base + Rust + React ya pisan ~6-8MB.
- **Target generoso `<50MB`** — rechazado: pierde ventaja vs Electron.

## Justificación
15MB es razonable para Tauri 2 + Rust + React + Tailwind + `git2` (que trae libssh2). Medir con `cargo bloat` (Rust) y `rollup-plugin-visualizer` (JS). Fallar build si >20MB.
