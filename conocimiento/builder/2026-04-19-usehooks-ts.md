# usehooks-ts — colección de hooks React

## TL;DR
- **usehooks-ts** (Julien Caron) = librería TypeScript-first de hooks React comunes, tree-shakeable.
- Incluye `useLocalStorage`, `useDebounceValue`, `useDebounceCallback`, `useMediaQuery`, `useInterval`, `useClickAnyWhere`, `useCopyToClipboard`, etc.
- ~30 hooks bien testeados, zero deps.

## Detalles

### Install
```bash
bun add usehooks-ts
```

### Hooks útiles para panel-agentes

**`useLocalStorage`** — persistir preferencias (tema, layout):
```tsx
const [theme, setTheme] = useLocalStorage<'dark' | 'light'>('theme', 'dark')
```

**`useDebounceValue`** — filtros de búsqueda:
```tsx
const [search, setSearch] = useState('')
const [debounced] = useDebounceValue(search, 300)
useEffect(() => filter(debounced), [debounced])
```

**`useInterval`** — polling periódico:
```tsx
useInterval(() => invoke('refresh_state'), 5000)
useInterval(callback, isActive ? 1000 : null)  // null pausa
```

**`useMediaQuery`** — responsive:
```tsx
const isMobile = useMediaQuery('(max-width: 768px)')
```

**`useCopyToClipboard`** — copiar IDs/comandos:
```tsx
const [copiedText, copy] = useCopyToClipboard()
<button onClick={() => copy(agent.id)}>Copiar</button>
```

**`useClickAnyWhere`** — cerrar dropdowns/modales:
```tsx
useClickAnyWhere(() => setMenuOpen(false))
```

**`useEventListener`** — wrapper tipado de addEventListener:
```tsx
useEventListener('keydown', handleKey)  // window default
useEventListener('scroll', onScroll, scrollRef)
```

### Breaking v3 (2025)
`useDebounce` fue reemplazado por 2 hooks explícitos:
- `useDebounceValue` — para valores
- `useDebounceCallback` — para funciones

Migración: revisar usos viejos, elegir el correcto por caso.

### Tree-shaking
```tsx
import { useLocalStorage, useDebounceValue } from 'usehooks-ts'
// solo estos 2 llegan al bundle
```
Sin wildcards. En ESM builder tree-shakea perfecto.

## Fuente
- https://usehooks-ts.com/
- https://github.com/juliencrn/usehooks-ts
- https://www.npmjs.com/package/usehooks-ts

## Aplicabilidad a panel-agentes
**Sí, default para utilidades.** Evita escribir 30 hooks custom. Usos concretos:
- `useLocalStorage` → tema, layout de paneles, última vista.
- `useInterval` → polling de STATE.md cada N segundos (fallback si file watcher Rust falla).
- `useDebounceValue` → input de búsqueda en command palette y tablas.
- `useCopyToClipboard` → copiar IDs de agente, paths, commits.
- `useMediaQuery` → ajustes responsive (aunque sea desktop, ventana chica pasa).
- `useEventListener` → atajos globales (⌘K, ⌘R, Esc).

Sin fricción, sin deps. Agregar al stack base.
