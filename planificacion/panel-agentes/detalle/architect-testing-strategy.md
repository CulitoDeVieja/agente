# Decisión: Testing strategy

**Decisión:** Rust: tests unitarios del parser markdown con fixtures (coverage alto ahí). JS/React: tests de integración del `useRepoData` hook + snapshot de `<AgentCard>`. Sin E2E v0.1.

## Alternativas
- **E2E con Playwright + Tauri driver** — rechazado v0.1: infra alta para 2 vistas. Sí para v0.2.
- **Coverage 100% todo** — rechazado: costo/beneficio pobre en componentes de presentación triviales.
- **Sin tests** — rechazado: parser markdown es el punto de falla probable (formato cambia).

## Justificación
Parser es la interfaz con formato externo → valor alto en tests. Componentes React son casi puros (reciben props, renderizan) → snapshot basta. `vitest` para JS (integra con Vite); `cargo test` nativo para Rust.
