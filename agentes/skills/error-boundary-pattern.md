# error-boundary-pattern

## Propósito
Capturar errores de render en componentes React y mostrar UI de fallback.

## Cuándo usar
Al envolver secciones de la UI que pueden fallar (datos externos, renders complejos).

## Pasos
1. Crear `ErrorBoundary.tsx`:
   ```tsx
   class ErrorBoundary extends React.Component<
     { children: React.ReactNode; fallback?: React.ReactNode },
     { hasError: boolean }
   > {
     state = { hasError: false };
     static getDerivedStateFromError() { return { hasError: true }; }
     render() {
       if (this.state.hasError) return this.props.fallback ?? <p>Error inesperado.</p>;
       return this.props.children;
     }
   }
   ```
2. Envolver componentes: `<ErrorBoundary><AgentPanel /></ErrorBoundary>`
3. Reset manual: agregar `key` prop para remontar: `<ErrorBoundary key={resetKey}>`

## Ejemplo
```tsx
<ErrorBoundary fallback={<div className="text-red-500">No se pudo cargar el panel.</div>}>
  <Dashboard />
</ErrorBoundary>
```

## Errores comunes
- Error Boundaries no capturan errores async (fetch, invoke) → manejar con try/catch en el hook.
- No reinicia al cambiar datos → usar `key` prop ligado al estado para forzar remontaje.
- Olvidar `getDerivedStateFromError` → sin él el error se propaga igual.
