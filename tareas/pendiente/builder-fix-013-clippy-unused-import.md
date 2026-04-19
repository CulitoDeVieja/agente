# Fix: `std::path::Path` unused import en parser.rs

**Rol:** builder
**Severidad:** baja
**Detectado por:** auditor-ops 3 → CI run 24635314737 (LUPA-018)
**Fecha:** 2026-04-19

## Problema
CI clippy falla con:
```
error: unused import: `std::path::Path`
  --> src/parser.rs:2:5
2 |     use std::path::Path;
```

Como CI corre con `clippy -- -D warnings`, esto rompe el build.

## Fix
```diff
- use std::path::Path;
```

(o si se usa en alguna firma, verificar y dejar.)

## AC
- [ ] Línea removida.
- [ ] CI rust-check pasa.
- [ ] Tests siguen 7/7.

## Depende de: (ninguna)
