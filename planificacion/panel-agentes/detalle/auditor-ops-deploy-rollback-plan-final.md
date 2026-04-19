# Plan auditoría/ops: deploy-rollback-plan-final

**Decisión:** Ver 04-auditoria.md §3. Disparadores y pasos ya definidos.

## Pasos concretos
1. `gh release delete v0.1.0 --yes` si compromete.
2. Reabrir `builder-*` con detalle del fallo.
3. Update STATE.md → bloqueado por incidente.
4. Crear `auditor-ops-*` post-mortem.

## Criterio pasa/no-pasa
Release retirado en <5min del disparador. Post-mortem en <24h.
