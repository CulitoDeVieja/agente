# React Compiler 1.0 — instalación en Vite 8

## TL;DR
- React Compiler es **1.0 estable** desde oct 2025: automemoiza componentes/hooks, elimina `useMemo`/`useCallback` manual.
- Con **Vite 8 + `@vitejs/plugin-react` v6**, Babel fue reemplazado por oxc → la instalación cambió.
- Hay que instalar `babel-plugin-react-compiler` + `@rolldown/plugin-babel` y usar el helper `reactCompilerPreset()`.

## Detalles

### Install
```bash
bun add -D babel-plugin-react-compiler @rolldown/plugin-babel
```

### Config `vite.config.ts` (Vite 8+, plugin-react v6)
```ts
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'
import { babel, reactCompilerPreset } from '@rolldown/plugin-babel'

export default defineConfig({
  plugins: [
    react(),
    babel({
      filter: /\.[jt]sx?$/,
      babelConfig: reactCompilerPreset(),
    }),
  ],
})
```

### Config anterior (Vite ≤7, plugin-react ≤5) — NO usar
```ts
react({ babel: { plugins: [['babel-plugin-react-compiler', {}]] } })  // ❌ ya no funciona
```

### Beneficios reales
- Adiós `useMemo`/`useCallback` manuales en 90% de los casos.
- Re-renders evitados automáticamente si las deps no cambiaron.
- Zero cambios en source code (es un compile-time transform).

### Gotchas
- Componentes que mutan props (anti-patrón) fallan el análisis → el compilador las salta.
- Regla dura: **Rules of React** deben cumplirse o el compilador no optimiza (hay ESLint plugin que lo detecta).
- Debugging: instalar React DevTools 6+ para ver qué componentes fueron compilados.

### ESLint
```bash
bun add -D eslint-plugin-react-compiler
```
Detecta violaciones a Rules of React que impiden optimización.

## Fuente
- https://react.dev/learn/react-compiler/installation
- https://react.dev/blog/2025/10/07/react-compiler-1
- https://dev.to/recca0120/react-compiler-10-vite-8-the-right-way-to-install-after-vitejsplugin-react-v6-drops-babel-p0i

## Aplicabilidad a panel-agentes
**Sí, desde día 1.** Plan Vite 8 + React 19 (decidido en evo-203). Adoptar React Compiler de entrada evita refactor después. Instalación: agregar `babel-plugin-react-compiler` + `@rolldown/plugin-babel` al `vite.config.ts`. Agregar `eslint-plugin-react-compiler` en el linter para detectar violaciones tempranas. Sin costo de migración porque arrancamos limpio.
