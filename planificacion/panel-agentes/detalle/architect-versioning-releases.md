# Decisión: Versioning y releases

**Decisión:** SemVer (`MAJOR.MINOR.PATCH`). v0.1.0 primera release (pre-estable). Releases como GitHub Releases con `.msi` + portable `.exe` + changelog. Sin auto-update en v0.1.

## Alternativas
- **CalVer (`2026.04.19`)** — rechazado: el proyecto tiene features discretos, no fechas arbitrarias.
- **Auto-update vía Tauri updater** — rechazado v0.1: firma de código requerida, costo alto para usuario único.
- **Sin releases, solo git tags** — rechazado: dificulta distribución del `.msi`.

## Justificación
SemVer es estándar. GitHub Releases son free y cubren downloads + changelog. Auto-update re-evaluar cuando la app tenga múltiples usuarios.

## Política
- `MAJOR` — breaking change en formato del repo `agente` (app vieja no lee repo nuevo).
- `MINOR` — feature nueva (create task, SSH trigger, etc).
- `PATCH` — bugfix o polish.
