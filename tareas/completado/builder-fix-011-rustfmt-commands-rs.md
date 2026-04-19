# Fix: `cargo fmt --check` falla en commands.rs

**Rol:** builder
**Severidad:** media
**Detectado por:** auditor-ops 3 → CI run 24635187055 (LUPA-016)
**Fecha:** 2026-04-19

## Problema
CI workflow nuevo (`.github/workflows/ci.yml`) falla en job `rust-check`:
```
cargo fmt --check
```
Diffs en `src-tauri/src/commands.rs` líneas 34, 43, 104, 111, 117, 143, etc.
Rustfmt quiere multi-line para signaturas largas, return-statements expandidos, args en bloques.

## Fix
```bash
cd src-tauri
cargo fmt
```
Y revisar el diff antes de commitear.

## AC
- [ ] CI rust-check pasa.
- [ ] `cargo fmt --check` exit 0 local.
- [ ] `npm test` sigue 7/7.

## Depende de: (ninguna — Builder lo arregla en próximo ciclo)
