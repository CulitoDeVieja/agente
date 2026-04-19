# Plan auditoría/ops: audit-tailwind-unused

**Decisión:** Regla hard: ningún warning/TODO en código que entra a main.

## Pasos concretos
1. `rg -n '(TODO|FIXME|XXX|HACK)' src/ src-tauri/src/`.
2. `rg -n ': any|as any' src/`.
3. `pnpm lint` (eslint).
4. `pnpm typecheck` (tsc --noEmit).
5. `cargo clippy --all-targets -- -D warnings`.

## Criterio pasa/no-pasa
Cero output de greps. Lint y typecheck verdes. Clippy sin warnings.
