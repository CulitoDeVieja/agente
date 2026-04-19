# Tauri 2 — Setup de auto-updater

## TL;DR
- Plugin oficial `@tauri-apps/plugin-updater` + `tauri-plugin-updater` (Rust).
- Se firma con par de llaves generado por `tauri signer generate`; privada en env `TAURI_SIGNING_PRIVATE_KEY`, pública en `tauri.conf.json`.
- Endpoint HTTP JSON devuelve `{ version, notes, pub_date, platforms }` con URL firmada del bundle.

## Detalles

### Instalación
```bash
bun add @tauri-apps/plugin-updater
cd src-tauri && cargo add tauri-plugin-updater
```

### Generar keys
```bash
bunx tauri signer generate -w ~/.tauri/panel-agentes.key
# exporta TAURI_SIGNING_PRIVATE_KEY + TAURI_SIGNING_PRIVATE_KEY_PASSWORD en CI
```

### Config (`tauri.conf.json`)
```json
"plugins": {
  "updater": {
    "pubkey": "<contenido de panel-agentes.key.pub>",
    "endpoints": ["https://releases.panel-agentes.dev/{{target}}/{{current_version}}"]
  }
}
```

### Frontend
```ts
import { check } from '@tauri-apps/plugin-updater'
const upd = await check()
if (upd?.available) { await upd.downloadAndInstall() }
```

### Endpoint JSON esperado
`{ version, pub_date, url, signature, notes }` por target (`windows-x86_64`, etc).

## Fuente
- https://v2.tauri.app/plugin/updater/
- https://github.com/tauri-apps/plugins-workspace/tree/v2/plugins/updater
- https://thatgurjot.com/til/tauri-auto-updater/

## Aplicabilidad a panel-agentes
**Sí, crítico.** App de escritorio para Tony en Windows — sin updater, cada fix requiere reinstalar manual. Plan: host del endpoint en GitHub Releases (gratis) con GitHub Action que firme con secret. Integrar en `builder-release-v0.1.0`. Private key en 1Password del equipo. Pendiente: decidir si hacer rollout gradual (10% → 100%) o full-push.
