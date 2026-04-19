# TanStack Query v5 — novedades y breaking changes

## TL;DR
- v5 unifica todas las firmas en un solo objeto: `useQuery({ queryKey, queryFn, ...options })`. Adiós a los overloads.
- `onSuccess/onError/onSettled` removidos de queries (quedan solo en mutations). `cacheTime → gcTime`, `isLoading → isPending`.
- Suspense first-class: `useSuspenseQuery`, `useSuspenseInfiniteQuery`, `useSuspenseQueries` devuelven data sin undefined.

## Detalles

**Breaking:**
- `useQuery(key, fn, opts)` → `useQuery({ queryKey, queryFn, ...opts })`. Lo mismo para `useInfiniteQuery`, `useMutation`, métodos de QueryClient.
- Estados: `status: "loading"` → `status: "pending"`. `isLoading` ahora combina `isPending && isFetching`.
- `keepPreviousData` → `placeholderData` con identity function.
- `cacheTime` → `gcTime` (más claro: garbage collection).
- `useErrorBoundary` → `throwOnError`.
- `isDataEqual` eliminado → usar `structuralSharing`.
- Prop `context` eliminada → pasar `queryClient` custom directamente.
- Infinite queries: `initialPageParam` obligatorio; `getNextPageParam` obligatorio.

**Features nuevos:**
- Hooks Suspense-first: devuelven data tipada sin undefined.
- Optimistic updates via `useMutation().variables` (sin tocar cache manualmente).
- `maxPages` en infinite queries → limita páginas en memoria.
- Multi-page prefetching con opción `pages`.
- `combine` en `useQueries` para agregados.

## Fuente
https://tanstack.com/query/latest/docs/framework/react/guides/migrating-to-v5

## Aplicabilidad a panel-agentes
**Sí** — si decidimos usar TanStack Query para fetching del estado del repo (tareas, STATE.md, etc.), v5 es la versión vigente. Hay que partir con las firmas nuevas (object-only). Los hooks Suspense simplifican el loading state cuando combinamos con React Suspense (ver skill `react-suspense-basics.md`). Alternativa más liviana: hook custom `useAsyncData` si el dataset es chico — TanStack Query es overkill para <5 queries totales.
