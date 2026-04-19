# Framer Motion (Motion) vs CSS animations

## TL;DR
- CSS animations = más rápidas (corren en compositor thread) y 0 KB JS. Suficiente para transiciones simples.
- **Motion** (ex framer-motion): ~32 KB gzip full, **17 KB hybrid** o **2.3 KB mini** (solo WAAPI) con tree-shaking (`LazyMotion`).
- Motion gana cuando necesitás: layout animations, AnimatePresence, springs físicos, gestos (drag/tap), o animaciones reactivas a estado.

## Detalles

### Cuándo CSS alcanza
- Hover/focus transitions (opacity, transform)
- Spinners, skeletons
- Enter/leave simples con keyframes + `animation-fill-mode`
- Panel-agentes side note: la mayoría de micro-interacciones caen acá.

### Cuándo Motion justifica bundle
- **Layout animations**: elementos que cambian de posición cuando reordenás lista (ej. agente sube de fila). CSS no puede, Motion con `layout` prop sí.
- **AnimatePresence**: elementos que aparecen/desaparecen con animación de salida (CSS con `transitionend` es frágil).
- **Springs**: física natural sin keyframes.
- **Drag/gestos**: drag-to-reorder con inercia.

### Tree-shaking agresivo
```tsx
import { LazyMotion, domAnimation, m } from 'motion/react'

<LazyMotion features={domAnimation}>
  <m.div animate={{ opacity: 1 }} />
</LazyMotion>
```
Usar `<m.div>` (no `<motion.div>`) y features lazy → 15 KB vs 32 KB.

### Mini runtime (2.3 KB)
```tsx
import { animate } from 'motion/mini'
animate('.agent-row', { opacity: [0, 1] }, { duration: 0.3 })
```
Solo CSS transitions vía WAAPI. Sin layout animations ni AnimatePresence. Ideal para widgets.

## Fuente
- https://motion.dev/docs/react-reduce-bundle-size
- https://motion.dev/blog/do-you-still-need-framer-motion
- https://blog.logrocket.com/best-react-animation-libraries/

## Aplicabilidad a panel-agentes
**Sí, pero con moderación.** Plan:
- **Default CSS** para: hover de filas, fade-in de loading, skeleton shimmer.
- **Motion (hybrid, lazy)** solo para: transición entre paneles (`AnimatePresence`), reordenamiento de lista de agentes (`layout`).
- **NO usar**: drag-to-reorder en v0.1 (no está en el scope), keyframes complejos.
- Presupuesto: ≤17 KB del bundle para Motion; si no lo justifica la UX, dejar solo CSS.
