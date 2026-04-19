# Dependabot para Rust (Cargo)

## TL;DR
- `.github/dependabot.yml` con `package-ecosystem: cargo` arma PRs automáticos para deps Rust.
- Usa Cargo.lock + RustSec advisory DB → alertas + PRs de bump.
- ⚠️ La imagen interna de Dependabot suele ir atrasada; falla en Rust 2024 edition con cargo <1.85.

## Detalles
Config mínima para panel-agentes:
```yaml
version: 2
updates:
  - package-ecosystem: cargo
    directory: "/src-tauri"      # raíz del Cargo.toml en Tauri
    schedule:
      interval: weekly
    open-pull-requests-limit: 5
    groups:
      tauri:
        patterns: ["tauri*"]
```

Combina con `cargo-audit` workflow: Dependabot bumpea versiones, audit valida que el bump no introduzca CVEs.

## Fuente
- https://github.blog/2022-06-06-github-brings-supply-chain-security-features-to-the-rust-community/
- https://github.com/dependabot/dependabot-core/issues/11691 (Rust 2024 edition gap)
- https://docs.rust-lang.org/cargo/reference/specifying-dependencies.html

## Aplicabilidad
**Sí** para panel-agentes (Tauri Rust). Activar después de auditor-ops-022 (CI). Agrupar deps de tauri para no tener PR ruido.
