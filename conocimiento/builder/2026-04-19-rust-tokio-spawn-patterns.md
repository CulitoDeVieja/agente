# Rust Tokio — patrones de spawn

## TL;DR
- `tokio::spawn` → tareas async independientes, devuelve `JoinHandle`.
- `spawn_blocking` → código sync/CPU-heavy o IO bloqueante; corre en pool separado; **no se puede abortar**.
- `JoinSet` → colectar N tareas heterogéneas y esperar cuando terminen (orden de completion).

## Detalles

### tokio::spawn — background async
```rust
let handle = tokio::spawn(async move {
    watch_agent_file(path).await
});
let res = handle.await??;
```

### spawn_blocking — para CPU/sync
```rust
let res = tokio::task::spawn_blocking(move || {
    // parsear 10k líneas de log sincrónicamente
    parse_huge_log(path)
}).await?;
```
Si lo usás para IO bloqueante, considerá `tokio::fs`/`tokio::process` primero.

### JoinSet — fan-out N tareas
```rust
let mut set = JoinSet::new();
for path in agent_files {
    set.spawn(async move { read_status(path).await });
}
while let Some(res) = set.join_next().await {
    let status = res??;
    // procesar a medida que termina
}
```
Ventaja vs `join_all`: podés cortar temprano, abortar todas, o streamear resultados.

### Regla rápida
| Caso | Usar |
|---|---|
| Varios futures, esperar todos | `JoinSet` / `try_join_all` |
| 1 task background que sobrevive al caller | `spawn` |
| Código bloqueante (sync IO, CPU) | `spawn_blocking` |
| Concurrencia pero mismo task | `tokio::join!` |

## Fuente
- https://docs.rs/tokio/latest/tokio/task/
- https://docs.rs/tokio/latest/tokio/task/struct.JoinSet.html
- https://tokio.rs/tokio/tutorial/spawning

## Aplicabilidad a panel-agentes
**Sí.** El backend Tauri va a: (a) watchear N archivos STATE.md/logs de agentes concurrentemente → `JoinSet` con un future por agente; (b) parsear markdown + detectar cambios de tabla → liviano, `tokio::spawn` alcanza; (c) si más adelante agregamos análisis pesado de logs (regex sobre GB), ahí sí `spawn_blocking`. No mezclar: usar `spawn_blocking` para IO bloqueante es anti-patrón cuando `tokio::fs` existe.
