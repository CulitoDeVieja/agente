# gitleaks — secrets scanning

## TL;DR
- SAST específico para detectar secretos hardcoded (API keys, tokens, passwords) en git history.
- Pre-commit hook + GitHub Action.
- Versión actual pre-commit: v8.24.2 (2026).

## Detalles

**Pre-commit hook (`.pre-commit-config.yaml`):**
```yaml
repos:
  - repo: https://github.com/gitleaks/gitleaks
    rev: v8.24.2
    hooks:
      - id: gitleaks
```

**GitHub Action:**
```yaml
- uses: gitleaks/gitleaks-action@v2
  env:
    GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
    # GITLEAKS_LICENSE: required SOLO para repos org privados
```

**Local scan history:**
```bash
gitleaks detect --source . -v             # commits actuales
gitleaks detect --source . --log-opts="--all"  # todo el history
```

Output formats: JSON, CSV, JUnit, SARIF (integra con GitHub Advanced Security).

vs Trivy: gitleaks es **especializado** en secrets, mejor signal-to-noise. Trivy es generalista. Combinar.

## Fuente
- https://github.com/gitleaks/gitleaks
- https://github.com/gitleaks/gitleaks-action
- https://oneuptime.com/blog/post/2026-01-25-secret-scanning-gitleaks/view

## Aplicabilidad
**Sí — alta prioridad.** auditor-ops-012 (grep credenciales hardcoded) se reemplaza por gitleaks. Agregar pre-commit hook + Action al panel-agentes.
