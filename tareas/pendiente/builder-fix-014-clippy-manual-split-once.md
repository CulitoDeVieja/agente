# Fix: clippy::manual_split_once en parser.rs:77

**Rol:** builder
**Severidad:** baja
**Detectado por:** auditor-ops 3 → CI run 24635445084 (LUPA-019)
**Fecha:** 2026-04-19

## Problema
CI clippy falla:
```
error: manual implementation of `split_once`
  --> src/parser.rs:77:9
77 | /         l.splitn(2, &pattern)
78 | |             .nth(1)
   | |___________________^ help: try: `l.split_once(&pattern).map(|x| x.1)`
```

Como CI corre con `clippy -- -D warnings`, esto rompe el build.

## Fix
```diff
- l.splitn(2, &pattern)
-     .nth(1)
+ l.split_once(&pattern).map(|x| x.1)
```

## AC
- [ ] Línea cambiada.
- [ ] CI rust-check pasa (esperado: sin errores clippy restantes).
- [ ] Tests siguen verdes.

## Depende de: (ninguna)
