# Fix: vitest config con `environment: jsdom`

**Rol:** builder
**Severidad:** media
**Detectado por:** auditor-ops (subagente audit dinámico) → bug LUPA-014

## Problema
`jsdom` está en `devDependencies` pero **no hay `vitest.config.ts`** que lo aplique con `test.environment: "jsdom"`. Cualquier test futuro de componentes React fallará por falta de DOM.

## Fix
Crear `vitest.config.ts`:
```ts
import { defineConfig } from "vitest/config";
import react from "@vitejs/plugin-react";

export default defineConfig({
  plugins: [react()],
  test: {
    environment: "jsdom",
    globals: true,
    setupFiles: ["./src/test/setup.ts"]
  },
});
```

Y `src/test/setup.ts` con `import "@testing-library/jest-dom";` si se usa testing-library.

## AC
- [ ] `vitest.config.ts` creado.
- [ ] `npm test` sigue 7/7.
- [ ] Test placeholder de componente React pasa con `render()`.

## Depende de: (ninguna)
