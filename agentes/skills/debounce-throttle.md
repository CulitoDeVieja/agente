# debounce-throttle

## Propósito
Limitar la frecuencia de ejecución de funciones en React usando debounce y throttle.

## Cuándo usar
- **Debounce**: búsqueda en tiempo real (esperar a que el usuario pare de escribir)
- **Throttle**: scroll handlers, resize, mousemove (limitar a N veces por segundo)
- Evitar llamadas excesivas a APIs o renders costosos

## Pasos

1. Debounce con hook propio:
   ```ts
   function useDebounce<T>(value: T, delay: number): T {
     const [debounced, setDebounced] = useState(value);

     useEffect(() => {
       const id = setTimeout(() => setDebounced(value), delay);
       return () => clearTimeout(id);
     }, [value, delay]);

     return debounced;
   }

   // Uso:
   const [query, setQuery] = useState('');
   const debouncedQuery = useDebounce(query, 300);

   useEffect(() => {
     if (debouncedQuery) search(debouncedQuery);
   }, [debouncedQuery]);
   ```

2. Debounce de función con `useCallback`:
   ```ts
   import { useMemo } from 'react';

   function useDebouncedCallback<T extends (...args: any[]) => any>(
     fn: T,
     delay: number
   ): T {
     const timeoutRef = useRef<ReturnType<typeof setTimeout>>();

     return useMemo(() => {
       return ((...args) => {
         clearTimeout(timeoutRef.current);
         timeoutRef.current = setTimeout(() => fn(...args), delay);
       }) as T;
     }, [fn, delay]);
   }

   const handleSearch = useDebouncedCallback(
     (q: string) => fetchResults(q),
     300
   );
   ```

3. Usando `usehooks-ts` o `lodash` (alternativa simple):
   ```bash
   npm install usehooks-ts
   ```
   ```ts
   import { useDebounceValue, useThrottle } from 'usehooks-ts';

   const [debouncedValue] = useDebounceValue(inputValue, 300);
   ```

4. Throttle manual:
   ```ts
   function useThrottledCallback<T extends (...args: any[]) => any>(fn: T, delay: number): T {
     const lastCall = useRef(0);
     return useMemo(() => ((...args) => {
       const now = Date.now();
       if (now - lastCall.current >= delay) {
         lastCall.current = now;
         fn(...args);
       }
     }) as T, [fn, delay]);
   }
   ```

## Ejemplo
Input de búsqueda: debounce 300ms evita llamar a la API en cada keystroke. Scroll handler: throttle 100ms limita el recálculo de posición a 10 veces/segundo.

## Errores comunes
- **Debounce sin limpiar**: usar siempre el return de `useEffect` para limpiar el timeout.
- **Función de debounce recreada en cada render**: envolver con `useCallback` o `useMemo`.
- **Confundir debounce con throttle**: debounce espera inactividad; throttle limita frecuencia máxima.
