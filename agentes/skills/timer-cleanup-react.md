# timer-cleanup-react

## Propósito
Gestionar y limpiar correctamente timers, intervals y efectos asíncronos en React con useEffect.

## Cuándo usar
- Polling periódico dentro de un componente
- Animaciones o contadores con setTimeout/setInterval
- Efectos que inician operaciones async que deben cancelarse al desmontar

## Pasos

1. Limpiar un `setTimeout`:
   ```ts
   useEffect(() => {
     const id = setTimeout(() => {
       setMessage('El tiempo expiró');
     }, 5000);

     return () => clearTimeout(id); // cleanup
   }, []);
   ```

2. Limpiar un `setInterval` (polling):
   ```ts
   useEffect(() => {
     const id = setInterval(() => {
       fetchStatus();
     }, 30_000);

     return () => clearInterval(id);
   }, []); // dep vacía = montar/desmontar
   ```

3. Cancelar operaciones async con flag:
   ```ts
   useEffect(() => {
     let cancelled = false;

     async function load() {
       const data = await fetchData();
       if (!cancelled) setData(data);
     }

     load();
     return () => { cancelled = true; };
   }, [id]);
   ```

4. Con AbortController (fetch cancelable):
   ```ts
   useEffect(() => {
     const controller = new AbortController();

     fetch('/api/tasks', { signal: controller.signal })
       .then(r => r.json())
       .then(setTasks)
       .catch(err => {
         if (err.name !== 'AbortError') setError(err);
       });

     return () => controller.abort();
   }, []);
   ```

5. Limpiar listener de eventos:
   ```ts
   useEffect(() => {
     const handler = (e: KeyboardEvent) => {
       if (e.key === 'Escape') onClose();
     };
     document.addEventListener('keydown', handler);
     return () => document.removeEventListener('keydown', handler);
   }, [onClose]);
   ```

## Ejemplo
Un componente de countdown que usa `setInterval` limpia el interval al desmontarse, evitando actualizar state en un componente ya eliminado.

## Errores comunes
- **"Can't perform a state update on unmounted component"**: falta de cleanup del timer o fetch.
- **Effect sin deps correcto**: si el interval usa una variable del scope, incluirla en las deps.
- **Múltiples intervals**: en StrictMode (React 18), el effect corre dos veces en dev; el cleanup previene doble interval.
