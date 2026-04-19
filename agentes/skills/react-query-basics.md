# react-query-basics

## Propósito
Fetching, caching y sincronización de datos del servidor en React con TanStack Query.

## Cuándo usar
- Datos que provienen de una API y necesitan cache y revalidación
- Evitar efectos manuales con `useEffect` + `useState` para fetching
- Necesidad de estados de loading/error automáticos

## Pasos

1. Instalar:
   ```bash
   npm install @tanstack/react-query
   ```

2. Configurar el QueryClient en el root:
   ```tsx
   import { QueryClient, QueryClientProvider } from '@tanstack/react-query';

   const queryClient = new QueryClient({
     defaultOptions: { queries: { staleTime: 1000 * 60 } },
   });

   function App() {
     return (
       <QueryClientProvider client={queryClient}>
         <Router />
       </QueryClientProvider>
     );
   }
   ```

3. Usar `useQuery` en un componente:
   ```ts
   import { useQuery } from '@tanstack/react-query';

   function TaskList() {
     const { data, isLoading, error } = useQuery({
       queryKey: ['tasks'],
       queryFn: () => fetch('/api/tasks').then(r => r.json()),
     });

     if (isLoading) return <Spinner />;
     if (error) return <ErrorBanner error={error} />;
     return <ul>{data.map(t => <li key={t.id}>{t.title}</li>)}</ul>;
   }
   ```

4. Usar `useMutation` para escritura:
   ```ts
   const mutation = useMutation({
     mutationFn: (task: NewTask) => fetch('/api/tasks', {
       method: 'POST', body: JSON.stringify(task),
     }),
     onSuccess: () => queryClient.invalidateQueries({ queryKey: ['tasks'] }),
   });
   ```

## Ejemplo
`queryKey: ['tasks', projectId]` — cambiar `projectId` recarga automáticamente los datos.

## Errores comunes
- **`queryKey` inconsistente**: siempre incluir todas las dependencias variables en el array.
- **Mutar sin invalidar**: olvidar `invalidateQueries` deja el cache desactualizado.
- **staleTime = 0 por defecto**: los datos se re-fetchen al enfocar la ventana; aumentar si son estables.
