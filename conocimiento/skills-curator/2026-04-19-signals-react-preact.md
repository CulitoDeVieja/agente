# Preact Signals en React

## TL;DR
- `@preact/signals-react` trae signals (fine-grained reactivity) a React sin cambiar a Preact/Solid.
- Componentes que leen un signal solo re-renderizan si ese signal cambió — no re-render del árbol completo.
- Requiere Babel plugin o `useSignals()` hook manual para activar tracking.

## Detalles

**Instalación:**
```bash
npm install @preact/signals-react
```

**Uso:**
```tsx
import { useSignal, useComputed } from "@preact/signals-react";

function Counter() {
  const count = useSignal(0);
  const doubled = useComputed(() => count.value * 2);

  return (
    <button onClick={() => count.value++}>
      {count.value} × 2 = {doubled.value}
    </button>
  );
}
```

**Concepts:**
- `useSignal(initial)` → crea signal; `.value` lee/escribe.
- `useComputed(fn)` → signal derivado.
- `useSignalEffect(fn)` → side effect cuando signals cambian (reemplaza `useEffect`).

**Activar tracking:**
Dos opciones:
1. **Babel transform** (`@preact/signals-react-transform`): auto-tracking en todos los components.
2. **Manual:** llamar `useSignals()` al inicio de cada component.

**Ventajas vs React state clásico:**
- Sin dep arrays en effects.
- Sin re-render cascadas: solo el nodo que lee el signal se actualiza.
- Sin `useMemo`/`useCallback` para stability — los signals ya son estables.

**Desventajas:**
- Requiere babel plugin (otra dep del build).
- Mixing `useState` y `useSignal` puede confundir.
- Compatibilidad con SSR/RSC todavía en iteración.

**Alternativa segura:** `@preact-signals/safe-react` — integra sin babel transform, más fricción pero menos magia.

## Fuente
- [Signals - Preact Guide](https://preactjs.com/guide/v10/signals/)
- [@preact/signals-react npm](https://www.npmjs.com/package/@preact/signals-react)
- [Better state management with Preact Signals - LogRocket](https://blog.logrocket.com/guide-better-state-management-preact-signals/)

## Aplicabilidad a panel-agentes
**Considerar para v0.2, no urgente.**

Comparación con lo que ya tenemos:
- `react-hooks-patterns.md` + `useAsyncData` → funcional pero requiere useCallback/useMemo manual.
- Si activamos **React Compiler** (evo 203), la memoization automática neutraliza parte del pitch de signals.

**Pro:** código más declarativo, menos `useEffect`/`useMemo`.
**Contra:** dep extra, mixing con hooks tradicionales, compatibilidad RSC.

**Decisión tentativa:** primero activar React Compiler (más standard), evaluar signals después si persisten re-renders problemáticos. Panel-agentes v0.1 es chico, no hay bottleneck visible.
