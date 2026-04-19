# Plan auditoría/ops: verificar-actualizaciones-no-pierden-config

**Decisión:** Config persiste durante update. Tauri updater (v0.2) conserva `%APPDATA%/panel-agentes/config.json`.

## Pasos concretos
1. Verificar que MSI no borra APPDATA durante upgrade.
2. Documentar location en README.

## Criterio pasa/no-pasa
Update v0.1.0 → v0.1.1 (test) mantiene config.
