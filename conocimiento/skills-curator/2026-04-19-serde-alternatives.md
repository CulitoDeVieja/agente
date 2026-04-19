# Serde alternatives en Rust — 2026

## TL;DR
- **Serde + serde_json** sigue siendo el default pragmático: madurez, ecosystem, derive macros.
- **rkyv** para performance extrema (zero-copy). Benchmarks lo ponen delante de bincode/postcard/flatbuffers.
- **miniserde / nanoserde** para reducir compile time (12x menos código generado que serde).

## Detalles

**Tabla rápida:**

| Lib | Caso ideal | Trade-off |
|---|---|---|
| serde + serde_json | JSON, APIs, config | baseline, compile-time lento |
| bincode | binario, inter-procesos | no self-describing |
| postcard | embedded, low-overhead | ~1.5x más lento que bincode, 70% del tamaño |
| rkyv | zero-copy, max perf | solo Rust-to-Rust, API más compleja |
| miniserde | JSON + compile-time rápido | menos features que serde |
| nanoserde | JSON/binary + sin macros | sintaxis distinta, sin ecosystem |

**Benchmarks (serialization, lower is better):**
- rkyv 0.8.10: ~235 µs
- nanoserde 0.2.1: ~233 µs
- serde_json: ~500-800 µs (baseline)

**rkyv ejemplo:**
```rust
use rkyv::{Archive, Deserialize, Serialize};

#[derive(Archive, Deserialize, Serialize)]
struct Task { id: String, estado: String }

let bytes = rkyv::to_bytes::<_, 256>(&task)?;
let archived = rkyv::check_archived_root::<Task>(&bytes[..])?;
```

**Cuándo NO migrar de serde:**
- Config files (JSON/TOML/YAML) → serde gana por comodidad.
- APIs HTTP → JSON es lingua franca.
- Inter-op con JS/Python → serde_json ganado.

## Fuente
- [rkyv is faster than bincode/capnp/cbor](https://david.kolo.ski/blog/rkyv-is-faster-than/)
- [Rust serialization benchmarks](https://github.com/djkoloski/rust_serialization_benchmark)
- [Rust serialization: production ready - LogRocket](https://blog.logrocket.com/rust-serialization-whats-ready-for-production-today/)

## Aplicabilidad a panel-agentes
**No cambiar** — serde es correcto para panel-agentes:
1. El panel parsea markdown + YAML frontmatter + JSON de estado. Todo text-based.
2. IPC Tauri usa JSON para Rust ↔ React — serde_json es la API oficial.
3. No hay bottleneck de performance (parseamos decenas de archivos, no millones).

**Skill existente correcta:** `rust-serde-json.md` cubre el caso principal.

**Nota para el futuro:** si algún día cacheamos un índice binario grande de tareas (ej. 10k tareas históricas), evaluar `rkyv` o `postcard`. No es la realidad actual.
