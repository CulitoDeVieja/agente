# Decisión: Signatures de comandos Rust

**Decisión:** 6 comandos `#[tauri::command]` que retornan `Result<T, String>`. Error como `String` (no enum) para simplificar serialización. Ver `01-arquitectura.md §3`.

## Alternativas
- **Error enum serializable** — rechazado v0.1: overhead para 5-6 variantes; `String` basta. Migrar a enum si v0.2 necesita clasificar por tipo (retry/toast/modal).
- **Async commands** — `git_pull` sí es async (red); lectura filesystem sincrónica es suficientemente rápida.
- **Batching de lecturas** — descartado: el parser se ejecuta en <50ms por dir; prematuro optimizar.

## Justificación
`Result<T, String>` mapea directo a `Promise<T>` en JS con `try/catch`. 6 comandos cubren v0.1: `list_tasks`, `read_state`, `read_task`, `git_pull`, `get_config`, `set_config`.
