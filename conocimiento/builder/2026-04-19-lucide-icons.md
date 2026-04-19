# lucide-react — íconos SVG tree-shakeable

## TL;DR
- **lucide-react** (fork mantenido de Feather): 1500+ íconos SVG inline, tree-shaking completo en prod.
- Cada ícono = componente standalone. Importar por nombre, solo lo usado va al bundle.
- **Caveat Vite dev**: no tree-shakea en dev mode, parece pesado mientras desarrollás.

## Detalles

### Install
```bash
bun add lucide-react
```

### Uso
```tsx
import { Activity, AlertCircle, Pause, Play, RefreshCw } from 'lucide-react'

<Activity size={16} className="text-green-500" />
<AlertCircle size={16} strokeWidth={2} />
```

### Tree-shaking correcto
- **Sí**: `import { Play } from 'lucide-react'` — solo ese ícono llega al bundle prod.
- **No**: `import * as Icons from 'lucide-react'` — rompe tree-shaking.

### Props útiles
- `size` (default 24) — px o `rem`.
- `strokeWidth` (default 2) — 1 para más fino, 2.5 para bolder.
- `color` / `className` — override de color.
- `absoluteStrokeWidth` — stroke no escala con size.

### Alternativas (2026)
| Lib | Íconos | Nota |
|---|---|---|
| Heroicons | 300+ | Ideal si ya usás Tailwind UI |
| Phosphor | 9000+, 6 weights | Más variedad, mayor bundle |
| Tabler | 5000+ | Consistencia alta, similar a lucide |
| Hugeicons | 46000+, 10 styles | Pesado, muchos son de pago |

Para panel-agentes (set chico y consistente), lucide es la elección default.

### Set mínimo sugerido para panel-agentes
`Activity, AlertCircle, CheckCircle2, Clock, Copy, Edit, FileText, GitBranch, Globe, Loader2, MoreHorizontal, Pause, Play, Power, RefreshCw, Search, Server, Settings, Trash2, X`.
~20 íconos = ~10 KB gzipped (inline SVG).

## Fuente
- https://lucide.dev/guide/packages/lucide-react
- https://medium.com/codetodeploy/the-hidden-bundle-cost-of-react-icons-why-lucide-wins-in-2026-1ddb74c1a86c
- https://javascript.plainenglish.io/tree-shaking-lucide-react-icons-with-vite-and-vitest-57bf4cfe6032

## Aplicabilidad a panel-agentes
**Sí, default de librería de íconos.** `bun add lucide-react`. Crear `src/icons.ts` que re-exporta solo los íconos que usamos — hace el bundle predecible y facilita reemplazos futuros:
```ts
export { Activity, AlertCircle, CheckCircle2, ... } from 'lucide-react'
```
Todos los componentes importan desde `@/icons`. Si shadcn/ui genera componentes con íconos hardcodeados, ya usa lucide-react por default → zero fricción.
