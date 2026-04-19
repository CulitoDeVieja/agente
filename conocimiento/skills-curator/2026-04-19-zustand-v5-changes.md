# Zustand v5 — breaking changes

## TL;DR
- Drops React <18, TS <4.5, ES5. Bundle más chico: usa `useSyncExternalStore` nativo en vez del polyfill.
- `create(...)` ya no acepta custom equality fn. Migración: `createWithEqualityFn` desde `zustand/traditional`.
- Selectores que retornan nueva referencia → warning/error. Solución: `useShallow` hook.

## Detalles

**Requisitos:**
- React ≥ 18 obligatorio.
- TypeScript ≥ 4.5.
- Ya no soporta target ES5.

**Bundle:** sin `use-sync-external-store` package → más liviano (usa API nativa de React 18).

**API cambios:**

v4:
```ts
const useStore = create(set => ({...}), shallow);
```

v5:
```ts
// Opción 1: migración mínima
import { createWithEqualityFn } from "zustand/traditional";
const useStore = createWithEqualityFn(set => ({...}), shallow);

// Opción 2: recomendada
import { create } from "zustand";
import { useShallow } from "zustand/shallow";

const useStore = create(set => ({...}));
const items = useStore(useShallow(s => s.items));
```

**Selectores:**
- Si un selector retorna nueva referencia cada render → infinite loop (en v4 era ineficiencia, en v5 es error).
- Fix: `useShallow` devuelve referencia estable si el shape es igual.
- Para objetos deep nested: `useDeep` custom hook (deep equality).

**Release:** v5.0.0 (2024-10). Activa en enero 2026.

## Fuente
- [Migrating to v5 - Zustand docs](https://zustand.docs.pmnd.rs/reference/migrations/migrating-to-v5)
- [Announcing Zustand v5 - Poimandres](https://pmnd.rs/blog/announcing-zustand-v5/)

## Aplicabilidad a panel-agentes
**Sí — si usamos Zustand.** Skill `zustand-vs-context-decision.md` ya existe. Si el Architect decide Zustand para panel-agentes:
1. Usar v5 desde el inicio (no hay razón para v4 en proyecto nuevo).
2. Setear el código desde el día 1 con `useShallow` para todos los selectores que retornen objetos/arrays.
3. Evitar `createWithEqualityFn` salvo migración legacy — `useShallow` es la API forward.

Actualizar skill existente: agregar ejemplo con `useShallow` para reemplazar el pattern viejo con `shallow` como segundo argumento.
