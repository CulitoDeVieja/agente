# logging-strategy

## Propósito
Definir qué y cómo loggear en la app Tauri (Rust + React) para debugging efectivo.

## Cuándo usar
Al agregar logging a comandos Rust o al frontend React.

## Pasos (Rust)
1. En `Cargo.toml`: `log = "0.4"`, `env_logger = "0.11"`
2. En `main.rs`: `env_logger::init();`
3. Usar: `log::info!("task loaded: {}", path); log::error!("failed: {}", e);`
4. Activar: `RUST_LOG=info cargo tauri dev`

## Pasos (React/Frontend)
1. Evitar `console.log` en producción → usar solo en dev guards:
   ```ts
   if (import.meta.env.DEV) console.log("debug:", data);
   ```
2. Para errores críticos en producción: escribir a archivo via command Rust.

## Ejemplo
```rust
log::info!("listing tasks in: {}", dir);
let entries = std::fs::read_dir(&dir).map_err(|e| {
    log::error!("read_dir failed: {}", e);
    e.to_string()
})?;
```

## Errores comunes
- `console.log` en producción → activar regla ESLint `no-console` para detectar.
- Logs sin contexto → incluir siempre qué operación y qué datos fallaron.
- Log level incorrecto → `info` para flujo normal, `warn` para recuperables, `error` para fallas reales.
