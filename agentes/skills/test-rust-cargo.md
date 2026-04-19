# test-rust-cargo

## Propósito
Escribir y ejecutar tests unitarios e de integración en Rust con `cargo test`.

## Cuándo usar
- Validar lógica de negocio en comandos Tauri
- Tests de funciones de utilidad (parsers, transformadores, validadores)
- Regresiones en CI para el backend Rust

## Pasos

1. Tests unitarios (mismo archivo, módulo `tests`):
   ```rust
   // src/tasks.rs
   pub fn priority_score(priority: &str) -> u8 {
       match priority {
           "high" => 3,
           "medium" => 2,
           "low" => 1,
           _ => 0,
       }
   }

   #[cfg(test)]
   mod tests {
       use super::*;

       #[test]
       fn test_priority_high() {
           assert_eq!(priority_score("high"), 3);
       }

       #[test]
       fn test_priority_unknown() {
           assert_eq!(priority_score("urgent"), 0);
       }

       #[test]
       #[should_panic]
       fn test_panic_example() {
           panic!("esto debería fallar");
       }
   }
   ```

2. Tests de integración en `tests/`:
   ```rust
   // tests/integration_test.rs
   use mi_crate::priority_score;

   #[test]
   fn integration_priority_medium() {
       assert_eq!(priority_score("medium"), 2);
   }
   ```

3. Correr tests:
   ```bash
   cargo test                     # todos
   cargo test priority            # filtrar por nombre
   cargo test -- --nocapture      # ver println! en output
   cargo test -- --test-threads=1 # serial (para tests con estado compartido)
   ```

4. Tests async con Tokio:
   ```rust
   #[tokio::test]
   async fn test_async_fetch() {
       let result = fetch_data().await;
       assert!(result.is_ok());
   }
   ```

5. Ignorar un test temporalmente:
   ```rust
   #[test]
   #[ignore = "requiere DB externa"]
   fn test_db_integration() { ... }
   ```

## Ejemplo
```bash
cargo test -- --nocapture 2>&1 | grep -E "(FAILED|ok)"
```

## Errores comunes
- **Tests con side effects globales**: usar `--test-threads=1` o aislar con mocks.
- **`unwrap()` en tests**: está bien en tests (panica con mensaje claro); no en código de producción.
- **Olvidar `#[cfg(test)]`**: sin esto, el módulo de test se compila en el binario final.
