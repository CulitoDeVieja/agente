# Plan auditoría/ops: release-workflow-draft

**Decisión:** CI opcional en v0.1: GitHub Actions con build+test en push a main.

## Pasos concretos
1. `.github/workflows/ci.yml` — test JS + Rust en push/PR.
2. `.github/workflows/release.yml` — on tag `v*`, build MSI + upload al release.
3. Secrets: ninguno en v0.1 (sin code signing aún).

## Criterio pasa/no-pasa
Actions verdes en un PR de prueba. Release workflow empaqueta `.msi` correctamente.
