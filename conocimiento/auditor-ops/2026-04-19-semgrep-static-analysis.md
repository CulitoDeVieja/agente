# Semgrep — SAST en CI

## TL;DR
- Engine OSS (LGPL-2.1) corre local + CI sin login, 2,800+ reglas community.
- AppSec Platform free hasta 10 contributors / 10 repos privados (20k+ pro rules).
- Soporta JS/TS/Python/Go/Rust/Java/C/etc; median scan 10s.

## Detalles
GitHub Actions mínimo:
```yaml
- uses: actions/checkout@v4
- uses: returntocorp/semgrep-action@v1
  with:
    config: >-
      p/default
      p/typescript
      p/react
      p/rust
      p/security-audit
```

Reglas custom (ejemplo: prohibir console.log en src/):
```yaml
# .semgrep/no-console.yml
rules:
  - id: no-console
    pattern: console.$METHOD(...)
    paths:
      include: ["src/"]
    message: "Usá logger, no console.*"
    severity: ERROR
```

Compite con CodeQL (free GitHub) y Snyk Code. Ventaja Semgrep: reglas custom triviales con sintaxis tipo el código.

## Fuente
- https://github.com/semgrep/semgrep
- https://semgrep.dev/products/community-edition/
- https://dev.to/rahulxsingh/is-semgrep-free-understanding-oss-vs-semgrep-cloud-in-2026-ge8

## Aplicabilidad
**Sí.** panel-agentes (TS+Rust) y futuros proyectos. Action gratis, scan 10s no rompe pipeline. Reemplaza varios audit checks manuales (auditor-ops-012 grep credenciales, 045 console.logs).
