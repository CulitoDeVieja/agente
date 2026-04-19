# local-cache-invalidation

## Propósito
Estrategias para invalidar y actualizar el cache local en apps offline-first o con sincronización.

## Cuándo usar
- Apps que cachean datos localmente y los sincronizan con un servidor
- Necesidad de mantener el cache fresco sin re-fetching innecesario
- Conflictos entre datos locales y datos del servidor

## Pasos

### Estrategias de invalidación

1. **Time-based (TTL)** — expirar el cache por tiempo:
   ```ts
   interface CacheEntry<T> {
     data: T;
     timestamp: number;
     ttl: number; // ms
   }

   function isCacheValid<T>(entry: CacheEntry<T>): boolean {
     return Date.now() - entry.timestamp < entry.ttl;
   }

   const cache = new Map<string, CacheEntry<unknown>>();

   async function getCached<T>(key: string, fetcher: () => Promise<T>, ttl = 5 * 60_000): Promise<T> {
     const entry = cache.get(key) as CacheEntry<T> | undefined;
     if (entry && isCacheValid(entry)) return entry.data;

     const data = await fetcher();
     cache.set(key, { data, timestamp: Date.now(), ttl });
     return data;
   }
   ```

2. **Event-based** — invalidar al recibir un evento de cambio:
   ```ts
   // Con TanStack Query:
   // Al recibir un evento de WS o SSE que indica cambio:
   wsClient.on('task-updated', (taskId) => {
     queryClient.invalidateQueries({ queryKey: ['tasks', taskId] });
   });

   // Invalidación en cascada:
   queryClient.invalidateQueries({ queryKey: ['tasks'] }); // invalida todo el prefijo
   ```

3. **Version-based** — usar un hash o versión para detectar cambios:
   ```ts
   async function checkForUpdates(localVersion: string) {
     const { version } = await fetch('/api/meta').then(r => r.json());
     if (version !== localVersion) {
       clearLocalCache();
       await syncFromServer();
     }
   }
   ```

4. **Stale-while-revalidate** — mostrar cache viejo mientras se refresca:
   ```ts
   // TanStack Query implementa esto con staleTime:
   useQuery({
     queryKey: ['tasks'],
     queryFn: fetchTasks,
     staleTime: 60_000,        // fresco por 1 minuto
     gcTime: 5 * 60_000,       // en cache 5 minutos
     refetchOnWindowFocus: true, // revalidar al volver a la app
   });
   ```

5. **Limpieza de cache obsoleto en SQLite local**:
   ```rust
   // Eliminar registros más viejos que 7 días
   conn.execute(
     "DELETE FROM cache WHERE created_at < datetime('now', '-7 days')",
     [],
   )?;
   ```

## Ejemplo
App de tareas offline: al reconectar, se invalida el cache de tareas y se re-fetcha del servidor; los cambios locales pendientes se sincronizan primero.

## Errores comunes
- **TTL demasiado largo**: datos stale que el usuario ve como actualizados — calibrar según frecuencia de cambio real.
- **Invalidar todo en cada cambio**: rendimiento pobre; invalidar solo el prefijo de query afectado.
- **Sin manejo de conflictos**: si hay cambios locales y del servidor simultáneos, decidir una estrategia (server-wins, client-wins, merge).
