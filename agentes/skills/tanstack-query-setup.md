# tanstack-query-setup

## Propósito
Configurar TanStack Query v5 (React Query) desde cero en un proyecto React + Vite.

## Cuándo usar
- Iniciar un proyecto nuevo que necesite fetching con cache
- Migrar de v4 a v5 de TanStack Query
- Configurar opciones globales de cache, retry y staleTime

## Pasos

1. Instalar:
   ```bash
   npm install @tanstack/react-query
   npm install -D @tanstack/react-query-devtools
   ```

2. Crear el `QueryClient` con opciones globales:
   ```ts
   // src/lib/queryClient.ts
   import { QueryClient } from '@tanstack/react-query';

   export const queryClient = new QueryClient({
     defaultOptions: {
       queries: {
         staleTime: 1000 * 60 * 5,    // 5 minutos
         gcTime: 1000 * 60 * 30,      // 30 minutos en cache
         retry: 1,
         refetchOnWindowFocus: false,
       },
       mutations: {
         retry: 0,
       },
     },
   });
   ```

3. Configurar el provider en `main.tsx`:
   ```tsx
   import { QueryClientProvider } from '@tanstack/react-query';
   import { ReactQueryDevtools } from '@tanstack/react-query-devtools';
   import { queryClient } from './lib/queryClient';

   ReactDOM.createRoot(document.getElementById('root')!).render(
     <QueryClientProvider client={queryClient}>
       <App />
       <ReactQueryDevtools initialIsOpen={false} />
     </QueryClientProvider>
   );
   ```

4. Patrón recomendado — query keys como constantes:
   ```ts
   // src/lib/queryKeys.ts
   export const queryKeys = {
     tasks: ['tasks'] as const,
     task: (id: string) => ['tasks', id] as const,
     projects: ['projects'] as const,
   };
   ```

5. Hook de query con tipado:
   ```ts
   import { useQuery } from '@tanstack/react-query';
   import { queryKeys } from '@/lib/queryKeys';

   export function useTasks() {
     return useQuery({
       queryKey: queryKeys.tasks,
       queryFn: () => fetch('/api/tasks').then(r => r.json()) as Promise<Task[]>,
     });
   }
   ```

## Ejemplo
Cambio de v4 a v5: `cacheTime` → `gcTime`; `onSuccess` en options eliminado, usar `useEffect`.

## Errores comunes
- **`cacheTime` en v5**: renombrado a `gcTime`; el build no falla pero la opción se ignora.
- **Devtools en producción**: importar con dynamic import o condición `import.meta.env.DEV`.
- **QueryClient creado dentro del componente**: siempre crear fuera del render para no re-instanciar.
