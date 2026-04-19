# react-suspense-basics

## Propósito
Usar React Suspense para mostrar loading states mientras cargan componentes o datos.

## Cuándo usar
Al hacer lazy loading de componentes o integrar con librerías que soportan Suspense.

## Pasos
1. Lazy loading de componentes:
   ```tsx
   const AgentDetail = React.lazy(() => import("./AgentDetail"));
   ```
2. Envolver con Suspense:
   ```tsx
   <Suspense fallback={<div>Cargando...</div>}>
     <AgentDetail />
   </Suspense>
   ```
3. Para datos async con TanStack Query u otra lib compatible: configurar `suspense: true` en la query.

## Ejemplo
```tsx
import { Suspense, lazy } from "react";
const HeavyChart = lazy(() => import("./HeavyChart"));

function Dashboard() {
  return (
    <Suspense fallback={<Spinner />}>
      <HeavyChart />
    </Suspense>
  );
}
```

## Errores comunes
- Suspense sin fallback → error en runtime; siempre proveer fallback.
- Lazy import en render → mover `lazy()` fuera del componente para evitar remontajes.
- No funciona con fetching manual (invoke) → Suspense para datos requiere librería compatible; usar `useAsyncData` hook manual en su lugar.
