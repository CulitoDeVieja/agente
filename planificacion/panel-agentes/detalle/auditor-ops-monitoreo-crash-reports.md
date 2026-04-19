# Plan auditoría/ops: monitoreo-crash-reports

**Decisión:** Monitoreo pasivo primera hora post-release: revisar issues creados por owner.

## Pasos concretos
1. `gh issue list --state open --label bug`.
2. Si hay crash, reabrir `auditor-ops-*` post-mortem.
3. Verificar ciclo de auditor-ops es idempotente (re-correr no rompe estado).

## Criterio pasa/no-pasa
Cero crashes en primera hora. Ciclo idempotente (2 runs seguidos = mismo resultado).
