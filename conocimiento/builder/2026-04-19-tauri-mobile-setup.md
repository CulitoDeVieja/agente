# Tauri 2 — Mobile setup (iOS + Android)

## TL;DR
- Tauri 2 (estable desde oct 2024) soporta iOS/Android nativamente con mismo codebase que desktop.
- Prerequisitos: **Rust + NDK Android + Xcode (macOS-only para iOS)**. Init: `tauri android init` / `tauri ios init`.
- HMR funciona en devices reales y emuladores — DX cercano a web.

## Detalles

### Prerequisitos Android
```bash
# Sistema
brew install android-studio   # o download desde developer.android.com
# Env vars
export ANDROID_HOME=$HOME/Library/Android/sdk
export NDK_HOME=$ANDROID_HOME/ndk/<version>
# Rust targets
rustup target add aarch64-linux-android armv7-linux-androideabi i686-linux-android x86_64-linux-android
```

### Prerequisitos iOS (solo macOS)
```bash
xcode-select --install           # NO alcanza, hace falta Xcode completo
sudo xcode-select -s /Applications/Xcode.app
rustup target add aarch64-apple-ios x86_64-apple-ios aarch64-apple-ios-sim
```

### Init proyecto mobile
```bash
# Desde root del proyecto Tauri
bunx tauri android init
bunx tauri ios init
```
Crea `src-tauri/gen/android/` y `src-tauri/gen/apple/` con Gradle/Xcode projects.

### Dev
```bash
bunx tauri android dev   # despliega en emulador/device
bunx tauri ios dev
```

### Diferencias con desktop
- **Plugins**: algunos plugins Rust solo desktop (ej. `window-vibrancy`). Usar feature flags.
- **Config separada**: `tauri.conf.json` tiene secciones `android`/`ios` para permisos, íconos, version code.
- **Bundle**: `tauri android build --apk` o `--aab` (Play Store), `tauri ios build` genera IPA.
- **Sign**: Android keystore / iOS provisioning profile configurados como env vars en CI.

### Caveats
- iOS build requiere cuenta Apple Developer ($99/año) para distribución.
- Android: dev sin cuenta, release en Play Store = $25 one-time.
- Bundle size mobile >20 MB típico (vs 5-10 MB desktop) por NDK runtime.

## Fuente
- https://v2.tauri.app/start/prerequisites/
- https://v2.tauri.app/develop/
- https://v2.tauri.app/blog/tauri-20/

## Aplicabilidad a panel-agentes
**No en v0.1.** Panel-agentes es Tauri Windows-only (monitorear agentes desde desktop de Tony). Mobile no aplica al use case. **Posible en v1.x:** app móvil para recibir push de alerts cuando un agente falla (ver en idle > 1h). Si se confirma esa necesidad, reutilizar mismo crate Rust con feature flag `mobile` y UI separada optimizada para táctil.
