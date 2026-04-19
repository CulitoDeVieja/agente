# GitHub Actions matrix — Windows builds para Tauri

## TL;DR
- `windows-latest` runner = última Windows Server (no Win 7/10/11 desktop).
- Para multi-versión de Windows: self-hosted runners por VM.
- `tauri-action` arma matrix multi-OS de una; default cubre 99% de casos.

## Detalles

**Workflow estándar Tauri (cubre Windows 10+ usando windows-latest):**
```yaml
strategy:
  matrix:
    platform: [windows-latest, macos-latest, ubuntu-22.04]
runs-on: ${{ matrix.platform }}
steps:
  - uses: actions/checkout@v4
  - uses: actions/setup-node@v4
    with: { node-version: 20 }
  - uses: dtolnay/rust-toolchain@stable
  - uses: tauri-apps/tauri-action@v0
    with:
      tagName: app-v__VERSION__
      releaseDraft: true
```

**Sobre Windows 7:** Tauri 2.x **NO soporta** Windows 7 (requiere WebView2 que solo está en Win 10 1803+). Si necesitás Win 7, quedarse en Tauri 1.x con WebView2 fallback.

**Para validar Win 11 específicamente:** runner `windows-2025` (cuando esté GA) o self-hosted con Win 11 instalado.

## Fuente
- https://v2.tauri.app/distribute/pipelines/github/
- https://github.com/tauri-apps/tauri-action
- https://remarkablemark.org/blog/2026/04/08/tauri-github-action/

## Aplicabilidad
**Sí — directo.** Workflow base para panel-agentes. Reemplaza auditor-ops-024 (build final Windows). Documentar que Win 7 no es target.
