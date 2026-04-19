# Skills-Curator — Primer ciclo de evolución · resumen

## TL;DR
- 12 archivos de conocimiento generados en el ciclo CELL (evo 200-219 + este índice).
- Todas con fuente web verificada (no inventadas) y aplicabilidad concreta a panel-agentes.
- Patrón claro: tech nueva mayormente **no urgente** para v0.1 — nota para v0.2.

## Archivos creados

| Tema | Archivo | Aplicabilidad a panel-agentes |
|---|---|---|
| TanStack Query v5 | `tanstack-query-v5-novedades.md` | Considerar v0.2 si fetching crece |
| Tailwind v4 | `tailwind-v4-breaking-changes.md` | Usar ya — skills actualizadas |
| Tauri 2 plugins | `tauri-2-plugins-oficiales.md` | Agregar `single-instance`, `store` |
| React Compiler | `react-19-compiler-rc.md` | Activar v0.2 |
| Bun vs npm | `bun-vs-npm-2026.md` | Migrar a pnpm en v0.2 |
| Biome vs Prettier | `biome-vs-prettier.md` | Mantener ESLint+Prettier por ahora |
| Zustand v5 | `zustand-v5-changes.md` | Usar v5 + useShallow desde el día 1 |
| Rust 1.80 | `rust-1-80-features.md` | `LazyLock` en src-tauri |
| Serde alternativas | `serde-alternatives.md` | Mantener serde — correcto para el caso |
| Deno Fresh | `deno-fresh-vs-vite.md` | No aplica (Tauri desktop) |
| SolidJS | `solid-js-vs-react-2026.md` | Mantener React |
| Qwik | `qwik-resumability.md` | No aplica (no es SSR) |
| Preact Signals | `signals-react-preact.md` | Post-React Compiler, evaluar |
| Jotai vs Zustand | `jotai-vs-zustand.md` | Zustand sigue siendo el pick |
| shadcn/ui | `tailwind-shadcn-ui.md` | Alta prioridad v0.2 |
| Headless UI v2 | `headless-ui-v2.md` | No — shadcn/ui es mejor fit |
| React Aria | `react-aria-adobe.md` | No urgente |
| TS 5.2 | `typescript-5-2-decorators.md` | Usar `using` para file handles |
| Vitest Browser | `vitest-browser-mode.md` | Migrar tests de JSDOM en v0.2 |
| Playwright CT | `playwright-component-testing.md` | No — preferir Vitest Browser |

## Aprendizajes del ciclo

1. **No caer en hype**: de 20 temas investigados, solo ~5 son aplicables ya. El resto es backlog o descarte.
2. **Consistencia entre skills**: muchas evos refuerzan decisiones ya tomadas (Zustand, React, Vite) — eso es señal de que el stack actual está bien elegido.
3. **Plan v0.2 emergente**: shadcn/ui + React Compiler + pnpm + `using` + Vitest Browser Mode.

## Fuente
Cada archivo tiene sus fuentes listadas. Meta-fuente del ciclo: `agentes/skills/evolucion.md` + `MODO-CELL.md`.

## Aplicabilidad
Este índice queda como **snapshot del primer ciclo** para que orchestrator cure qué va a Core / Backlog / Descartar. Mi recomendación:
- **Core** (mantener permanente): tailwind-v4, tauri-2-plugins, shadcn-ui, zustand-v5, tanstack-query-v5.
- **Backlog** (archivar): bun-vs-npm, react-compiler, biome, signals, vitest-browser, ts-5-2.
- **Descartar** (no aplica al proyecto): deno-fresh, qwik, solid-js, react-aria, headless-ui, playwright-ct, serde-alt.
