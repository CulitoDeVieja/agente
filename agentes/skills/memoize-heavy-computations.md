# memoize-heavy-computations

## Propósito
Evitar recalcular operaciones costosas en cada render usando `useMemo` y `useCallback` en React.

## Cuándo usar
- Filtrar o ordenar listas largas (>500 ítems)
- Cálculos derivados complejos (agregaciones, estadísticas)
- Funciones pasadas como props a componentes memorizados

## Pasos

1. `useMemo` para cálculos derivados:
   ```ts
   // Sin memoización — recalcula en cada render
   const filtered = tasks.filter(t => t.priority === selectedPriority);

   // Con useMemo — recalcula solo si tasks o selectedPriority cambian
   const filtered = useMemo(
     () => tasks.filter(t => t.priority === selectedPriority),
     [tasks, selectedPriority]
   );
   ```

2. Ordenamiento costoso:
   ```ts
   const sortedTasks = useMemo(() => {
     return [...tasks].sort((a, b) => {
       if (sortBy === 'priority') return priorityScore(b) - priorityScore(a);
       if (sortBy === 'dueDate') return new Date(a.dueDate) < new Date(b.dueDate) ? -1 : 1;
       return a.title.localeCompare(b.title);
     });
   }, [tasks, sortBy]);
   ```

3. `useCallback` para funciones estables:
   ```ts
   const handleDelete = useCallback((id: string) => {
     dispatch({ type: 'DELETE_TASK', id });
   }, [dispatch]); // solo se recrea si dispatch cambia
   ```

4. Memoizar componentes con `memo`:
   ```tsx
   const TaskRow = memo(function TaskRow({ task, onDelete }) {
     return (
       <div>
         {task.title}
         <button onClick={() => onDelete(task.id)}>Eliminar</button>
       </div>
     );
   });
   // onDelete debe ser estable (useCallback) para que memo sea efectivo
   ```

5. Cuándo NO usar useMemo:
   ```ts
   // Innecesario — la operación es trivial
   const doubled = useMemo(() => count * 2, [count]); // no vale la pena

   // Innecesario — el array siempre tiene 3 ítems
   const items = useMemo(() => [1, 2, 3], []); // usar fuera del componente
   ```

## Ejemplo
Filtrar y ordenar una lista de 3.000 tareas toma ~40ms; con `useMemo`, solo recalcula cuando cambian las deps, no en cada keystroke del formulario adyacente.

## Errores comunes
- **Deps incompletas**: olvidar una dependencia hace que el memo use valores stale.
- **Memoizar todo**: `useMemo` tiene overhead propio; solo usarlo cuando el cálculo es measurablemente lento.
- **`useCallback` sin `memo` en el hijo**: `useCallback` solo tiene sentido si el componente hijo usa `memo` o `useEffect` con la función como dep.
