# optimistic-updates

## Propósito
Actualizar la UI inmediatamente antes de confirmar la operación en el servidor para una experiencia más ágil.

## Cuándo usar
- Marcar una tarea como completada (esperamos que funcione)
- Like/unlike, toggle de estado
- Operaciones rápidas donde el fallo es poco probable

## Pasos

1. Patrón manual con useState:
   ```ts
   function TaskItem({ task }: { task: Task }) {
     const [done, setDone] = useState(task.done);
     const [saving, setSaving] = useState(false);

     const toggle = async () => {
       const prevDone = done;
       setDone(!done); // actualización optimista

       try {
         await updateTask(task.id, { done: !done });
       } catch {
         setDone(prevDone); // rollback si falla
         toast.error('No se pudo guardar el cambio');
       }
     };

     return (
       <input type="checkbox" checked={done} onChange={toggle} />
     );
   }
   ```

2. Con TanStack Query (recomendado):
   ```ts
   const queryClient = useQueryClient();

   const mutation = useMutation({
     mutationFn: (update: Partial<Task>) => updateTask(task.id, update),

     onMutate: async (update) => {
       await queryClient.cancelQueries({ queryKey: ['tasks'] });

       // Snapshot del estado anterior (para rollback)
       const previous = queryClient.getQueryData<Task[]>(['tasks']);

       // Actualización optimista del cache
       queryClient.setQueryData<Task[]>(['tasks'], old =>
         old?.map(t => t.id === task.id ? { ...t, ...update } : t)
       );

       return { previous };
     },

     onError: (_err, _update, context) => {
       // Rollback
       queryClient.setQueryData(['tasks'], context?.previous);
       toast.error('Error al guardar');
     },

     onSettled: () => {
       queryClient.invalidateQueries({ queryKey: ['tasks'] });
     },
   });
   ```

3. Uso desde el componente:
   ```ts
   <input
     type="checkbox"
     checked={task.done}
     onChange={() => mutation.mutate({ done: !task.done })}
   />
   ```

## Ejemplo
Checkbox que se marca visualmente al instante; si la API falla, vuelve al estado anterior con un toast.

## Errores comunes
- **Sin rollback**: una actualización optimista sin manejo de error deja la UI en estado incorrecto.
- **No cancelar queries en curso**: `cancelQueries` en `onMutate` evita que una respuesta tardía sobreescriba la actualización optimista.
- **Operaciones destructivas sin confirmación**: no hacer optimistic delete sin un mecanismo de undo visible.
