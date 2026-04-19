# error-tracking-sentry

## Propósito
Capturar y monitorear errores en producción en apps React + Tauri con Sentry.

## Cuándo usar
- Detectar errores que los usuarios encuentran pero no reportan
- Obtener stack traces completos de errores en producción
- Monitorear performance y errores con contexto de usuario

## Pasos

1. Instalar:
   ```bash
   npm install @sentry/react
   ```

2. Inicializar Sentry en `main.tsx`:
   ```ts
   import * as Sentry from '@sentry/react';

   Sentry.init({
     dsn: import.meta.env.VITE_SENTRY_DSN,
     environment: import.meta.env.MODE,
     release: import.meta.env.VITE_APP_VERSION,
     enabled: import.meta.env.PROD,
     tracesSampleRate: 0.1, // 10% de transacciones
     integrations: [
       Sentry.browserTracingIntegration(),
       Sentry.replayIntegration({ maskAllText: true }),
     ],
   });
   ```

3. Wrap del router para tracing:
   ```tsx
   const SentryRoutes = Sentry.withSentryReactRouterV6Routing(Routes);
   ```

4. Error Boundary con Sentry:
   ```tsx
   function App() {
     return (
       <Sentry.ErrorBoundary
         fallback={({ error, resetError }) => (
           <ErrorFallback error={error} onRetry={resetError} />
         )}
       >
         <Router />
       </Sentry.ErrorBoundary>
     );
   }
   ```

5. Capturar errores manualmente:
   ```ts
   try {
     await riskyOperation();
   } catch (err) {
     Sentry.captureException(err, {
       tags: { module: 'export' },
       extra: { userId, exportFormat },
     });
   }
   ```

6. Agregar contexto de usuario:
   ```ts
   Sentry.setUser({ id: user.id, email: user.email });
   // Al cerrar sesión:
   Sentry.setUser(null);
   ```

## Ejemplo
Un error de `TypeError: cannot read properties of null` en producción aparece en Sentry con el stack trace, el usuario afectado y las últimas acciones (breadcrumbs).

## Errores comunes
- **DSN expuesto en repo**: usar `VITE_SENTRY_DSN` en `.env` y agregar `.env` a `.gitignore`.
- **`enabled: true` en desarrollo**: genera ruido en el dashboard; usar `import.meta.env.PROD`.
- **Source maps faltantes**: sin source maps, los stack traces apuntan al código minificado; usar `sentry-vite-plugin`.
