# SolidJS vs React — 2026

## TL;DR
- Solid: fine-grained reactivity (signals), sin virtual DOM. ~70% más rápido, bundle 6x menor (5KB vs 40KB).
- React: virtual DOM, re-renders componentes enteros. Ecosystem masivo (190M vs 0.5M weekly downloads).
- Solid mejor para apps con muchas updates frecuentes; React mejor para equipos + ecosystem.

## Detalles

**Performance (js-framework-benchmark 2026):**
- Solid: cerca de vanilla JS.
- React: significativamente detrás, especialmente en listas grandes y árboles profundos.
- Solid renderiza componentes 2-3x más rápido en benchmarks estándar.

**Modelo reactivo:**

**Solid (signals):**
```ts
const [count, setCount] = createSignal(0);
// Solo el nodo DOM bound a `count` se actualiza al cambiar.
```

**React (VDOM):**
```tsx
const [count, setCount] = useState(0);
// El componente entero re-renderiza; React reconcilia el VDOM.
```

**Bundle:**
- Solid core: ~5KB gzipped.
- React + ReactDOM: ~40KB gzipped.

**Ecosystem:**
- React: 230K+ stars, 190M weekly downloads, TODO el ecosystem (UI libs, hooks, routers, auth).
- Solid: creciente, ~500K weekly. Solid Start para fullstack. Sin el peso de React.

**Sintaxis:**
- Solid JSX se parece a React pero los components corren **una sola vez** (no cada render). Signals controlan updates.
- Esto cambia patterns: sin `useEffect`-hell, sin `useMemo`/`useCallback` (Solid no necesita memoization — no re-renderiza).

## Fuente
- [SolidJS vs React 2026 Performance - BoundDev](https://www.boundev.com/blog/solidjs-vs-react-2026-performance-guide)
- [React vs Solid.js 2026 - PkgPulse](https://www.pkgpulse.com/blog/react-vs-solidjs-2026)
- [State of Solid.js in 2026](https://listiak.dev/blog/the-state-of-solid-js-in-2026-signals-performance-and-growing-influence)

## Aplicabilidad a panel-agentes
**No migrar — mantener React.**

Razones:
1. **Ecosystem:** panel-agentes ya tiene 20+ skills React (tanstack-query, zustand, tailwind, suspense, error-boundary, etc.). Reescribir desde cero es waste.
2. **React Compiler:** activando el Compiler (evo 203), mucha de la ventaja de Solid en memoization automática se neutraliza.
3. **Tamaño del panel:** no tenemos bottleneck de performance. El panel maneja ~100-500 tareas, no hay listas gigantes.

**Cuándo reconsiderar Solid:**
- Si hacemos UI con streams en tiempo real (monitoring panel con 10k updates/seg).
- Para un proyecto greenfield nuevo donde no hay inercia React.

Para skills-curator: **conocer Solid por cultura general**, pero no invertir en skills específicas hasta que haya un proyecto que lo requiera.
