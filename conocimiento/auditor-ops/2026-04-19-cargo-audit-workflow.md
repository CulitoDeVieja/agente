# cargo-audit en GitHub Actions

## TL;DR
- `cargo-audit` revisa Cargo.lock contra la **RustSec Advisory DB** y rompe el build si hay CVEs.
- Action recomendada (oficial Rust org): `actions-rust-lang/audit` o `rustsec/audit-check`.
- Configurable vía `audit.toml`; ignora advisories informativos.

## Detalles
Workflow típico para panel-agentes (Tauri = backend Rust):

```yaml
name: security-audit
on:
  push: { paths: ["**/Cargo.toml", "**/Cargo.lock"] }
  schedule: [{ cron: "0 5 * * *" }]   # diario 05:00 UTC
jobs:
  audit:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: rustsec/audit-check@v2
        with: { token: ${{ secrets.GITHUB_TOKEN }} }
```

Disparadores recomendados:
1. Cambios en `Cargo.toml`/`Cargo.lock` (paths filter).
2. Schedule diario para detectar nuevos CVEs sin commits.

Salida: si hay vulnerabilidad → status check rojo + opcional creación de issue por CVE.

## Fuente
- https://github.com/actions-rust-lang/audit
- https://github.com/rustsec/audit-check
- https://rustsec.org/

## Aplicabilidad
**Sí.** panel-agentes usa Tauri (Rust). Add este workflow al repo `panel-agentes` apenas tenga CI configurado (auditor-ops-022). Bloquea releases con CVEs y avisa de nuevos advisories sin esfuerzo manual.
