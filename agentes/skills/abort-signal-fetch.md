# abort-signal-fetch

## Propósito
Cancelar requests HTTP y operaciones async con AbortController y AbortSignal en JavaScript/TypeScript.

## Cuándo usar
- Cancelar una búsqueda cuando el usuario sigue escribiendo (debounce + abort)
- Cancelar requests al desmontar un componente React
- Timeouts personalizados para fetch

## Pasos

1. Cancelar fetch básico:
   ```ts
   const controller = new AbortController();

   fetch('/api/search?q=hola', { signal: controller.signal })
     .then(r => r.json())
     .then(console.log)
     .catch(err => {
       if (err.name === 'AbortError') {
         console.log('Request cancelado');
       }
     });

   // Cancelar:
   controller.abort();
   ```

2. Búsqueda con cancelación de request anterior:
   ```ts
   let currentController: AbortController | null = null;

   async function search(query: string) {
     currentController?.abort(); // cancelar el anterior
     currentController = new AbortController();

     try {
       const res = await fetch(`/api/search?q=${query}`, {
         signal: currentController.signal,
       });
       return res.json();
     } catch (err) {
       if ((err as Error).name === 'AbortError') return null;
       throw err;
     }
   }
   ```

3. En React con useEffect:
   ```ts
   useEffect(() => {
     const controller = new AbortController();

     fetch(`/api/tasks/${id}`, { signal: controller.signal })
       .then(r => r.json())
       .then(setTask)
       .catch(err => { if (err.name !== 'AbortError') setError(err); });

     return () => controller.abort();
   }, [id]);
   ```

4. Timeout con AbortSignal (nativo desde 2022):
   ```ts
   const res = await fetch('/api/data', {
     signal: AbortSignal.timeout(5000), // cancela en 5 segundos
   });
   ```

5. Combinar timeout + cancelación manual:
   ```ts
   const controller = new AbortController();
   const timeoutId = setTimeout(() => controller.abort(), 5000);

   try {
     const res = await fetch(url, { signal: controller.signal });
     clearTimeout(timeoutId);
     return res.json();
   } catch (err) {
     clearTimeout(timeoutId);
     throw err;
   }
   ```

## Ejemplo
Input de búsqueda: cada keystroke cancela el fetch anterior y lanza uno nuevo, evitando race conditions.

## Errores comunes
- **No distinguir AbortError de otros errores**: siempre verificar `err.name === 'AbortError'` antes de mostrar error al usuario.
- **AbortController reutilizado**: una vez que se llama `abort()`, el controller está "muerto"; crear uno nuevo para cada request.
- **Sin cleanup en useEffect**: sin `controller.abort()` en el return, el fetch actualiza state de un componente desmontado.
