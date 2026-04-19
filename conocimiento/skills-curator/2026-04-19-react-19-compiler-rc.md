# React Compiler (v19) — RC y v1.0 estable

## TL;DR
- React Compiler 1.0 ya es stable. Reemplaza `useMemo`/`useCallback`/`React.memo` manuales — el compiler los genera automático.
- Se instala como babel plugin: `npm install --save-dev --save-exact babel-plugin-react-compiler`.
- En Vite: agregar al config de `vite-plugin-react` como babel plugin. Rolldown/oxc support en roadmap.

## Detalles

**Estado 2026:** v1.0 release oficial en 2025-10-07. RC previo fue 2025-04. Listo para producción.

**Qué resuelve:**
- Memoización automática: no hace falta envolver callbacks con `useCallback`, ni cálculos con `useMemo`, ni componentes con `React.memo`.
- El compiler detecta estáticamente qué re-renders son innecesarios y los elide.

**Instalación (Vite):**
```bash
npm install --save-dev --save-exact babel-plugin-react-compiler
```

En `vite.config.ts`:
```ts
import react from "@vitejs/plugin-react";
export default defineConfig({
  plugins: [
    react({
      babel: {
        plugins: [["babel-plugin-react-compiler", {}]]
      }
    })
  ]
});
```

**Adopción incremental:** se puede activar por directorio o por archivo (opt-in) con config. Permite migración gradual sin forzar reescritura.

**Limitaciones:**
- Requiere código que siga las rules-of-react (ESLint plugin `eslint-plugin-react-compiler` detecta violaciones).
- No optimiza side effects — solo renderings puros.

## Fuente
- [React Compiler v1.0](https://react.dev/blog/2025/10/07/react-compiler-1)
- [Installation](https://react.dev/learn/react-compiler/installation)

## Aplicabilidad a panel-agentes
**Sí — recomendado para v0.2.** Panel-agentes está en v0.1 con React 19. Activar el compiler:
1. Permite borrar la skill `memoize-heavy-computations.md` del flujo diario (el compiler lo hace solo).
2. Simplifica `react-hooks-patterns.md` — menos `useCallback` manual.
3. No urgente para v0.1 (panel chico, sin perf issues). Incluir en backlog de v0.2.

**Cuidado:** activar ESLint plugin `eslint-plugin-react-compiler` junto con el compiler para detectar violaciones de rules-of-react antes de que el compiler las salte.
