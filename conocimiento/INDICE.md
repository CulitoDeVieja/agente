# Índice de conocimiento — curado post-CELL

**Última consolidación:** 2026-04-19 · por orchestrator (tarea orchestrator-010)
**Total archivos:** 61 (20 + 21 + 20)

---

## 🟢 CORE — queda permanente (usamos ya o en próximas 2 semanas)

### Skills-Curator
- `tailwind-v4-breaking-changes.md` — stack actual, crítico.
- `tauri-2-plugins-oficiales.md` — stack actual.
- `typescript-5-2-decorators.md` — TS es nuestro frontend.
- `vitest-browser-mode.md` — testing frontend.
- `tanstack-query-v5-novedades.md` — si agregamos queries async.
- `tailwind-shadcn-ui.md` — diseño coherente con mockup v4.
- `zustand-v5-changes.md` — si decidimos state manager.
- `react-aria-adobe.md` — accesibilidad (audit cubre).
- `playwright-component-testing.md` — E2E futuro v0.2.
- `biome-vs-prettier.md` — tooling DX.

### Builder
- `tauri-react-ts-ipc-patterns.md` — **core absoluto**, primer evo.
- `tauri-updater-setup.md` — v0.2 feature confirmada.
- `tauri-icons-generator.md` — release v0.1 necesita icon.
- `rust-thiserror-vs-anyhow.md` — error handling Rust.
- `rust-tokio-spawn-patterns.md` — async Rust.
- `lucide-icons.md` — iconografía UI.
- `sonner-toasts.md` — toasts en mockup v4.
- `cmdk-command-menu.md` — cmd palette en mockup v4.
- `tanstack-table-v8.md` — tabla tareas en panel.
- `react-error-boundary-lib.md` — error boundary planificado.
- `radix-ui-primitives-v2.md` — base de shadcn/ui.
- `usehooks-ts.md` — hooks reutilizables.
- `vite-6-rolldown.md` — bundler.
- `day-picker-v9.md` — si hay filtro por fecha.

### Auditor-Ops
- `bundle-analyzer-tauri.md` — audit bundle target <15MB.
- `cargo-audit-workflow.md` — security deps Rust.
- `code-signing-free-2026.md` — release Windows.
- `lighthouse-ci-setup.md` — métricas performance.
- `signtool-alternatives.md` — release Windows alternatives.
- `windows-smartscreen-trust.md` — release Windows gate.
- `nsis-vs-wix-installer.md` — installer decision.
- `osv-scanner-google.md` — vulns scan.
- `github-actions-matrix-windows.md` — CI futuro.
- `github-dependabot-rust.md` — updates automáticos.

## 🟡 BACKLOG — útil en futuro (archivar si se acumula)

### Skills-Curator
- `solid-js-vs-react-2026.md` — solo si cambiamos framework.
- `qwik-resumability.md` — solo si migramos web.
- `signals-react-preact.md` — optimización futura.
- `jotai-vs-zustand.md` — solo si state crece.
- `headless-ui-v2.md` — alternativa shadcn.
- `react-19-compiler-rc.md` — esperar estable.
- `bun-vs-npm-2026.md` — migración tooling.
- `deno-fresh-vs-vite.md` — no aplica desktop.
- `rust-1-80-features.md` — referencia.
- `serde-alternatives.md` — serde funciona.

### Builder
- `react-server-components-desktop.md` — no aplica Tauri desktop.
- `framer-motion-vs-css.md` — CSS suficiente v0.1.
- `motion-one-lib.md` — alternativa framer.
- `react-compiler-install.md` — esperar estable.
- `tauri-mobile-setup.md` — v0.3+ expansion.
- `tauri-window-vibrancy.md` — polish v0.2.
- `rust-axum-embedded.md` — no hay backend HTTP.

### Auditor-Ops
- `gitleaks-secrets.md` — backup de osv-scanner.
- `trivy-scan.md` — alternativa a cargo-audit.
- `semgrep-static-analysis.md` — nice to have.
- `snyk-free-tier.md` — pago, esperar.
- `squirrel-windows-updater.md` — alternativa Tauri updater.
- `velopack-installer.md` — alternativa NSIS.
- `trunk-check-tool.md` — alternativa Biome.
- `renovate-vs-dependabot.md` — dependabot elegido.
- `renovate-config-presets.md` — si cambiamos de dependabot.
- `windows-defender-exclusion.md` — caso borde.

## 🔴 DESCARTAR — sin valor identificable

Ninguno. Todos los 61 archivos tienen fuente real y aplicabilidad aunque sea futura.

---

## Regla a partir de ahora

Antes de crear skill, componente o tarea nueva:
```bash
grep -r "<palabra-clave>" conocimiento/<tu-rol>/
```
Si hay resultado → reusá el conocimiento archivado en vez de reinventar.
