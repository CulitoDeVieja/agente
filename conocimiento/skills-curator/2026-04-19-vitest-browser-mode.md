# Vitest Browser Mode — 2026

## TL;DR
- Vitest Browser Mode: tests de componentes en browser real (no JSDOM). Elimina la necesidad de JSDOM/HappyDOM.
- Providers: Playwright (recomendado) o WebdriverIO. Vitest no trae automation — delega.
- Web APIs reales funcionan out-of-the-box: localStorage, cookies, fetch, clipboard, web workers, etc.

## Detalles

**Setup:**
```bash
npm install -D vitest @vitest/browser @vitest/browser-playwright playwright
```

`vite.config.ts`:
```ts
export default defineConfig({
  test: {
    browser: {
      enabled: true,
      provider: "playwright",
      instances: [
        { browser: "chromium" },
        { browser: "firefox" },  // opcional
      ],
    },
  },
});
```

**Test de componente:**
```ts
import { render } from "vitest-browser-react";
import { expect, test } from "vitest";

test("AgentCard muestra nombre", async () => {
  const { getByText } = render(<AgentCard name="builder" />);
  await expect.element(getByText("builder")).toBeVisible();
});
```

**vs JSDOM/HappyDOM:**
| | Vitest Browser | JSDOM |
|---|---|---|
| Runtime | browser real | simulación DOM |
| Web APIs | todas reales | subset incompleto |
| Speed | más lento (100-500ms/test) | rapidísimo |
| CI | necesita Playwright binaries | nada |
| Debug visual | sí (preview mode) | no |

**vs Playwright E2E:**
| | Vitest Browser Mode | Playwright E2E |
|---|---|---|
| Alcance | componente aislado | app completa |
| Unidad | component | page/flow |
| Velocidad | rápido | más lento |
| Use case | unit + integration | end-to-end |

Ambos son complementarios — Vitest Browser para components, Playwright E2E para flujos completos.

## Fuente
- [Vitest Browser Mode docs](https://vitest.dev/guide/browser/)
- [Vitest Browser Mode vs Playwright - Epic Web](https://www.epicweb.dev/vitest-browser-mode-vs-playwright)
- [Why Browser Mode - Vitest](https://vitest.dev/guide/browser/why)

## Aplicabilidad a panel-agentes
**Sí — considerar migrar de JSDOM a Browser Mode en v0.2.**

Panel-agentes actual: skill `react-testing-vitest.md` usa JSDOM. Razones para migrar:
1. Panel usa Tauri `invoke()` — mockearlo en JSDOM es frágil. En browser real podemos mock a nivel más alto.
2. Algunos components usan Clipboard API (copiar task ID) → JSDOM no lo soporta bien.
3. Virtualización de listas (si agregamos `react-virtual`) → requiere layout real, JSDOM no mide.

**Plan v0.2:**
- Agregar `@vitest/browser` + Playwright como dev deps.
- Migrar tests de componentes críticos primero (AgentCard, Dashboard).
- Mantener tests de hooks/utils en JSDOM (más rápidos).
- E2E con Playwright standalone para flujos completos (deploy, monitoreo).

**No urgente v0.1:** 6 tests actuales en JSDOM pasan bien. Migrar cuando agreguemos features que toquen APIs no-JSDOM.
