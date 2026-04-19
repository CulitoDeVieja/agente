# Plan: rust-command-list-tasks

## Objetivo
Implementar comando Tauri `list_tasks(estado, rol)` en Rust.

## Signature
```rust
#[tauri::command]
fn list_tasks(estado: &str, rol: &str) -> Vec<Task>
```

## Pasos
1. Leer directorio `tareas/{estado}/` del repo local
2. Filtrar archivos que empiezan con `{rol}-` (o todos si rol == "all")
3. Parsear cada `.md`: extraer `id`, `title`, `priority`, `dependeDe`
4. Retornar Vec serializable a JSON

## Tests planeados
- [ ] Retorna lista vacía si directorio no existe
- [ ] Filtra correctamente por rol
- [ ] Parsea título y prioridad del frontmatter
