# Motion One — librería liviana WAAPI

## TL;DR
- **Motion One** = runtime animation framework-agnostic sobre Web Animations API (WAAPI). Bundle: **3.8 KB** (vs 17 KB `animate()` de framer-motion).
- Ideal para performance/tamaño, especialmente mobile o widgets.
- Limitación: WAAPI "black box" — no podés leer el valor del animation mid-frame.

## Detalles

### Diferencia vs framer-motion/Motion (full)
| Aspecto | Motion One | framer-motion (Motion react) |
|---|---|---|
| Bundle | 3.8 KB | 17-32 KB |
| Target | Vanilla JS / React wrapper | React first |
| Engine | WAAPI | mix WAAPI + rAF JS |
| Layout animations | ❌ | ✅ |
| AnimatePresence | ❌ (hay workaround) | ✅ |
| Gestos | limitado | completo |
| Read values mid-frame | ❌ | ✅ |

### Uso básico
```js
import { animate } from 'motion'

animate('.agent-row', { opacity: [0, 1], y: [10, 0] }, { duration: 0.3 })
animate('.toast', { scale: [0.9, 1] }, { easing: 'spring(1, 100, 10, 0)' })
```

### React wrapper
```tsx
import { motion } from 'motion/react'
<motion.div animate={{ opacity: 1 }} initial={{ opacity: 0 }} />
```
Importando de `motion/react` en vez de `framer-motion` — la lib ahora se llama **Motion** y ex-framer-motion es el package.

### Cuándo elegir Motion One puro (no React wrapper)
- Animar elementos fuera de React tree (ej. toasts de `sonner`, notificaciones nativas).
- Widget/embed donde cada KB importa.
- Animaciones simples: fade, scale, translate, rotate.

### Cuándo subir a full Motion (React)
- Necesitás `layout` prop.
- AnimatePresence para exit animations.
- Gestos drag/tap complejos.
- Lectura de motion values en render.

## Fuente
- https://motion.dev/blog/should-i-use-framer-motion-or-motion-one
- https://blog.logrocket.com/exploring-motion-one-framer-motion/
- https://www.reactlibraries.com/blog/framer-motion-vs-motion-one-mobile-animation-performance-in-2025

## Aplicabilidad a panel-agentes
**Sí, si adoptamos animation lib.** Complemento de evo-207: para panel-agentes las animaciones son simples (fade in/out, scale del modal, shimmer de skeleton). Motion One (3.8 KB) alcanza para 80% de los casos. Plan:
- `bun add motion` — usar `motion/mini` o import directo de `animate()`.
- Reservar full Motion (`motion/react` con features lazy) solo si aparece necesidad de `layout` animation o AnimatePresence.
- Si el bundle lo permite, un solo import de Motion hybrid (17 KB) simplifica el mental model vs tener dos libs.
