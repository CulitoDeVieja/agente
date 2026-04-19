# Plan auditoría/ops: audit-security-deps

**Decisión:** Scan automatizado + manual antes de cada release.

## Pasos concretos
1. `rg -in '(api[_-]?key|token|password|secret|bearer)' src/ src-tauri/src/`.
2. `cargo audit` (Rust deps CVEs).
3. `pnpm audit --audit-level=high` (JS deps).

## Criterio pasa/no-pasa
Cero matches con valores reales (no solo type signatures). `cargo audit` sin vulnerabilidades. `pnpm audit` sin 'high' o 'critical'.
