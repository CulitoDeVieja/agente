# Plan auditoría/ops: audit-licenses-spdx

**Decisión:** Auditar deps cada release. Rechazar copyleft en deps de runtime.

## Pasos concretos
1. `pnpm dlx depcheck` (deps JS no usadas).
2. `cargo machete` (deps Rust no usadas).
3. `pnpm licenses list` y `cargo license` — filtrar por GPL/AGPL en runtime.

## Criterio pasa/no-pasa
Cero deps unused reportadas. Cero licencias GPL/AGPL en runtime (dev-deps ok).
