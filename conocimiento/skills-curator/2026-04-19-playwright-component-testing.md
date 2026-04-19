# Playwright Component Testing — 2026

## TL;DR
- Playwright Component Testing es stable en 2026. `@playwright/test` + `mount()` — tests de componentes en browser real.
- Diferencia clave vs Vitest Browser Mode: Playwright serializa JSX y reconstruye en browser (bridge). Vitest corre directamente en browser.
- Para proyectos Vite + Vitest existentes: **Vitest Browser Mode** es más natural. Playwright CT es mejor si ya usás Playwright E2E.

## Detalles

**Setup:**
```bash
npm init playwright@latest -- --ct
```

**Estructura generada:**
```
playwright/
  index.html       ← host de los components
  index.ts         ← setup global
playwright.config.ts
```

**Test:**
```tsx
import { test, expect } from "@playwright/experimental-ct-react";
import { AgentCard } from "./AgentCard";

test("muestra nombre del agente", async ({ mount }) => {
  const component = await mount(<AgentCard name="builder" />);
  await expect(component.getByText("builder")).toBeVisible();
});
```

**Comando:**
```bash
npx playwright test -c playwright-ct.config.ts
```

**vs Vitest Browser Mode:**

| | Playwright CT | Vitest Browser |
|---|---|---|
| Arquitectura | serializa JSX → browser | corre test en browser |
| Speed | rápido, paralelo | comparable |
| Hot reload | no (re-mount) | sí (Vite HMR) |
| Mockeo fino | limitado | completo (vi.mock) |
| Import CSS | manual setup | nativo Vite |
| Debug visual | UI mode Playwright | preview mode |
| Integration con E2E | mismo runner | distinto runner |

**Cuándo usar Playwright CT:**
- Ya tenés E2E con Playwright → unificás config y reports.
- Querés correr components + E2E en misma suite CI.
- No usás Vite.

**Cuándo usar Vitest Browser:**
- Ya usás Vitest para unit tests.
- Vite es tu bundler.
- Querés HMR durante desarrollo de tests.

## Fuente
- [Vitest Browser Mode vs Playwright - Epic Web](https://www.epicweb.dev/vitest-browser-mode-vs-playwright)
- [Vitest Browser Mode vs Playwright CT - PkgPulse 2026](https://www.pkgpulse.com/blog/vitest-browser-mode-vs-playwright-component-testing-vs-2026)
- [Playwright CT GitHub issue #34819](https://github.com/microsoft/playwright/issues/34819)

## Aplicabilidad a panel-agentes
**No — preferir Vitest Browser Mode (evo 218).**

Razones:
1. Panel-agentes ya usa Vitest (skill `react-testing-vitest.md`). Cambiar a Playwright CT sería migración grande sin ganancia.
2. Vite es el bundler — Vitest Browser integra nativo.
3. No tenemos E2E todavía → sin leverage de "un solo runner".

**Caso donde reconsiderar:** si en v0.3 agregamos E2E completos de flujo (panel → edit tarea → commit → verificar en otro agente), podría convenir unificar todo en Playwright (CT + E2E). Pero eso es futuro lejano.

**Regla actual:**
- Componentes aislados → Vitest Browser Mode (plan v0.2).
- Flujos completos → Playwright E2E standalone.
- Hooks y utils puros → Vitest + JSDOM (más rápido).
