# Plan auditoría/ops: ops-release-v010-final

**Decisión:** Release final coordinado: tag + build + upload + release notes + owner notification en ese orden.

## Pasos concretos
1. Todos los auditor-ops tests verdes (20-52).
2. `git tag v0.1.0` + push.
3. Trigger release workflow (o manual: build + `gh release create`).
4. Upload binarios y notas.
5. Log + notificar owner.

## Criterio pasa/no-pasa
Release publicado, MSI descargable, hash verificable.
