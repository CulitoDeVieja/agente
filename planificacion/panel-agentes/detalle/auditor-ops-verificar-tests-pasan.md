# Plan auditoría/ops: verificar-tests-pasan

**Decisión:** Tests obligatorios pre-release.

## Pasos concretos
1. `cargo test` (Rust).
2. `pnpm test` (vitest).
3. Revisar coverage del parser markdown.

## Criterio pasa/no-pasa
Ambas suites verdes. Parser con >=5 fixtures pasando.
