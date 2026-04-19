# Tauri 2 + React + TS — Patrones IPC tipados

## TL;DR
- Tauri 2 expone `invoke()` (tipo fetch) para comandos Rust desde React/TS; eventos vía `listen()`/`emit()`.
- Para type-safety real end-to-end, usar **TauRPC** (genera tipos TS desde Rust en runtime) o **tauri-bindgen** (bindings declarativos vía `.wit`).
- Los comandos deben ser JSON-serializables; eventos no son tipados y no devuelven valor.

## Detalles

### Patrón nativo Tauri 2
```ts
import { invoke } from '@tauri-apps/api/core'
const res = await invoke<AgentStatus>('get_agent_status', { id: 1 })
```
Rust:
```rust
#[tauri::command]
async fn get_agent_status(id: u32) -> Result<AgentStatus, String> { ... }
```
Limitación: tenés que duplicar tipos en TS manualmente.

### TauRPC (recomendado si el repo tiene >10 comandos)
- Declarás traits en Rust con `#[taurpc::procedures]`.
- Genera automáticamente cliente TS tipado en runtime.
- Soporta eventos bidireccionales tipados.
- Zero duplicación de tipos.

### tauri-bindgen
- Más formal: contrato en archivos `.wit` (WebAssembly Interface Types).
- Compila bindings Rust + TS/JS.
- Mejor si el equipo quiere un contrato versionado como API pública.

## Fuente
- https://v2.tauri.app/concept/inter-process-communication/
- https://github.com/MatsDK/TauRPC
- https://github.com/tauri-apps/tauri-bindgen

## Aplicabilidad a panel-agentes
**Sí** — el panel va a exponer muchos comandos (listar agentes, leer STATE.md, lanzar loops, leer logs). Duplicar tipos Rust↔TS a mano es deuda segura. **Recomendación:** adoptar TauRPC desde el comando #5. Para los primeros 4 comandos, `invoke<T>()` nativo con `src-tauri/bindings.ts` manual es suficiente.
