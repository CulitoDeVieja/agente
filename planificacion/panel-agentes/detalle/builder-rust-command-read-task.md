# Plan: rust-command-read-task

## Objetivo
Implementar comando `read_task(archivo)` que devuelve detalle de una tarea.

## Signature
```rust
#[tauri::command]
fn read_task(archivo: &str) -> TaskDetail
```

## Pasos
1. Leer archivo `.md` en `tareas/{estado}/{archivo}`
2. Extraer secciones: título, rol, prioridad, AC, Depende de, Log del agente
3. Parsear checkboxes `- [x]` vs `- [ ]`
4. Retornar `TaskDetail` serializable

## Tests planeados
- [ ] Extrae título correctamente
- [ ] Parsea checkboxes AC completos e incompletos
- [ ] Log vacío devuelve string vacío
