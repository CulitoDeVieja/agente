# Rust 1.80 — features clave

## TL;DR
- `LazyLock` / `LazyCell`: lazy initialization de statics con init function incluida. Reemplaza el pattern `OnceLock::new()` + fn helper.
- Exclusive range patterns: `a..b` en match arms (antes solo `a..=b`).
- Lints nuevos: `non_contiguous_range_endpoints` y `overlapping_range_endpoints` para evitar off-by-one.

## Detalles

**Release:** 1.80.0 — 2024-07-25. Versión mainline en 2026.

**LazyLock (thread-safe):**
```rust
use std::sync::LazyLock;

static CONFIG: LazyLock<Config> = LazyLock::new(|| {
    serde_json::from_str(include_str!("config.json")).unwrap()
});

fn get_port() -> u16 { CONFIG.port }
```

**LazyCell (single-thread):**
```rust
use std::cell::LazyCell;

thread_local! {
    static CACHE: LazyCell<HashMap<u32, String>> = LazyCell::new(HashMap::new);
}
```

- `LazyLock` reemplaza `OnceLock` + fn init externa.
- `LazyCell` no es `Sync`; solo en thread_local o contextos single-thread.

**Exclusive range patterns:**
```rust
match score {
    0..50 => "fail",          // antes: 0..=49
    50..80 => "pass",
    80..=100 => "great",
    _ => "invalid",
}
```

**Lints para migrar seguro:**
- `non_contiguous_range_endpoints` — detecta si hay gap entre ranges.
- `overlapping_range_endpoints` — detecta si dos ranges se solapan.

## Fuente
- [Announcing Rust 1.80.0](https://blog.rust-lang.org/2024/07/25/Rust-1.80.0/)
- [Rust 1.80 release notes](https://releases.rs/docs/1.80.0/)

## Aplicabilidad a panel-agentes
**Sí — usar en src-tauri/.** Rust 1.80+ ya está disponible. Recomendado:
1. `LazyLock` para config global del panel (leer una sola vez): path del repo agente, preferencias de usuario, cache de filesystem.
2. Exclusive ranges en match de estados de tarea (pendiente=0..10, en-curso=10..20, completado=20..100, etc.).

**No obligatorio**: panel v0.1 es chico, la ganancia de `LazyLock` vs `OnceLock::get_or_init` es principalmente cosmética. Priorizar en código nuevo, no refactor.

Actualizar `Cargo.toml`: `edition = "2021"` sigue ok, solo verificar que rust-toolchain apunte a `>=1.80`.
