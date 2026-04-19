# Plan auditoría/ops: build-final-windows

**Decisión:** `cargo tauri build --target x86_64-pc-windows-msvc` genera MSI + portable .exe.

## Pasos concretos
1. `pnpm install --frozen-lockfile`.
2. `pnpm build`.
3. `cargo tauri build --target x86_64-pc-windows-msvc`.
4. Verificar artefactos en `src-tauri/target/release/bundle/`.

## Criterio pasa/no-pasa
`.msi` + `-setup.exe` presentes. Ambos <15MB. Instalan y desinstalan limpios en VM.
