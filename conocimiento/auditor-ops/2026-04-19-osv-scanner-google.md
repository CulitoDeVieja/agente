# OSV-Scanner (Google)

## TL;DR
- CLI Go free + open source (Apache 2.0), sin login ni límites.
- Soporta Cargo, npm, pip, go, maven, etc. — usa OSV.dev como DB.
- v2.3.5 (mar/2026): scan transitivo en Python requirements.txt vía deps.dev.

## Detalles
Local:
```bash
osv-scanner scan source -r .            # recursive
osv-scanner scan source -L Cargo.lock   # solo Rust
osv-scanner fix --strategy in-place     # remediation guiada (v2)
```

GitHub Action:
```yaml
- uses: google/osv-scanner-action/.github/workflows/osv-scanner-reusable.yml@v2.3.5
  with:
    scan-args: |-
      -r
      ./
```

vs `cargo-audit`: OSV es multi-lenguaje (un tool para Cargo + npm de Tauri); cargo-audit es solo Rust pero más profundo en RustSec advisories. **Usar ambos en paralelo** — overhead ≈ 0.

## Fuente
- https://github.com/google/osv-scanner
- https://google.github.io/osv-scanner/
- https://security.googleblog.com/2025/03/announcing-osv-scanner-v2-vulnerability.html

## Aplicabilidad
**Sí.** Cubre frontend (npm) + backend (Cargo) en un solo workflow para panel-agentes. Complementa cargo-audit (no reemplaza). Agregar al CI junto con cargo-audit.
