# Velopack — installer + auto-updater moderno

## TL;DR
- Sucesor de Squirrel.Windows. Escrito en **Rust**, cross-platform (Win/macOS/Linux).
- Updates en ~2 segundos, **sin UAC prompts**.
- "Zero config" — un comando genera installer + delta updates + portable + self-update.

## Detalles

```bash
dotnet tool install -g Velopack
vpk pack \
  --packId panel-agentes \
  --packVersion 0.1.0 \
  --packDir ./publish \
  --mainExe panel-agentes.exe
```

Genera:
- `panel-agentes-Setup.exe` (installer NSIS-like)
- `panel-agentes-Portable.zip` (no install)
- `panel-agentes-0.1.0-full.nupkg` (paquete update)
- `panel-agentes-0.1.0-delta.nupkg` (delta update)
- `RELEASES` file (manifiesto)

**Migración desde Squirrel:** CLI casi idéntico, los usuarios existentes se actualizan a Velopack en un release.

## vs Tauri plugin-updater
| Criterio | Velopack | tauri-plugin-updater |
|---|---|---|
| Native a Tauri | ❌ | ✅ |
| Delta updates | ✅ | ❌ (full download) |
| Multi-platform | ✅ | ✅ |
| UAC-free | ✅ (per-user install) | ✅ |
| Install + update unified | ✅ | ❌ (install separado) |

Para apps Tauri grandes (>50MB) donde el delta importa: **Velopack puede valer la pena** vs tauri-updater. Para apps chicas: tauri-updater alcanza.

## Fuente
- https://github.com/velopack/velopack
- https://velopack.io
- https://docs.velopack.io/

## Aplicabilidad
**Reservar.** panel-agentes v0.1.0 con tauri-updater alcanza (binario chico). Si en v1.x el binario crece >50MB y hay updates frecuentes → migrar a Velopack vale la pena por delta.
