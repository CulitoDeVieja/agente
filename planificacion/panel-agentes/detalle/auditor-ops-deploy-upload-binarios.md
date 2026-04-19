# Plan auditoría/ops: deploy-upload-binarios

**Decisión:** Release via `gh release create` con tag SemVer.

## Pasos concretos
1. `git tag -a v0.1.0 -m 'panel-agentes v0.1.0'`.
2. `git push --tags`.
3. `gh release create v0.1.0 --title 'v0.1.0' --notes-file release-notes-v0.1.0.md`.
4. `gh release upload v0.1.0 *.msi *-setup.exe`.

## Criterio pasa/no-pasa
Release visible en `gh release view v0.1.0` con los 2 binarios. Tag presente en remote.
