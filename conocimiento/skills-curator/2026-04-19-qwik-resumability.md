# Qwik — resumability vs hydration

## TL;DR
- Qwik serializa el estado del framework en HTML → no ejecuta JS al cargar, "resume" desde el server.
- Sin hidratación: TTI sub-100ms incluso en 3G vs 200-500ms de hidratación clásica.
- Payload JS: **O(1)** (constante) — no escala con tamaño de app.

## Detalles

**Hidratación clásica (React, Vue, Svelte):**
Al cargar una página SSR, el framework debe:
1. Restaurar listeners.
2. Reconstruir component tree.
3. Restaurar estado de aplicación.

Esto requiere descargar y ejecutar **toda la lógica de la app** antes de que sea interactiva → 200-500ms extra.

**Resumability (Qwik):**
- El server serializa no solo datos sino **framework state + listener registry** al HTML.
- Botones ya reaccionan al click antes de ejecutar ningún JS.
- JS se carga **lazy**, solo del listener específico que se clickea.

**Ejemplo:**
```tsx
// Qwik component
import { component$, useSignal, $ } from "@builder.io/qwik";

export const Counter = component$(() => {
  const count = useSignal(0);
  return <button onClick$={$(() => count.value++)}>{count.value}</button>;
});
```

El `$` marca el boundary de lazy loading — Qwik solo descarga el handler onClick cuando el usuario clickea.

**Benefits:**
- TTI constante sin importar tamaño app.
- Primera interacción < 100ms.
- Bundle dividido automáticamente en chunks por listener.

**Trade-offs:**
- Ecosystem chico comparado con React.
- Debugging más complejo (chunks lazy cambian según interacción).
- Curva de aprendizaje: `$` boundaries y `useSignal` son conceptos nuevos.

## Fuente
- [Qwik Resumable concept docs](https://qwik.dev/docs/concepts/resumable/)
- [Resumability vs Hydration - builder.io](https://www.builder.io/blog/resumability-vs-hydration)
- [Unraveling Qwik's Resumability - Leapcell](https://leapcell.io/blog/unraveling-qwik-s-resumability-to-eliminate-hydration-overhead)

## Aplicabilidad a panel-agentes
**No.** Mismo caso que Fresh (evo 209): panel-agentes es Tauri desktop, no app web SSR.

Qwik tiene sentido para:
- Sitios públicos con muchos visitantes (landing pages, e-commerce).
- Apps donde TTI en móvil 3G es crítico.
- Content heavy con interacción puntual.

**No aplica** a Tauri desktop — no hay latencia de red al cargar la app (archivos locales), todo JS es instant.

**Nota cultural:** conocer resumability por si alguna vez hacemos una landing del proyecto o un dashboard público del estado del sistema de agentes.
