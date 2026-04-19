# virtualization-tasks-list

## Propósito
Virtualizar una lista de tareas larga en React con @tanstack/virtual para mantener 60fps.

## Cuándo usar
- Lista de tareas con más de 500 ítems
- Scroll lento o janky en listas largas
- Necesidad de mantener performance en proyectos con muchos registros

## Pasos

1. Instalar:
   ```bash
   npm install @tanstack/react-virtual
   ```

2. Componente de lista virtualizada de tareas:
   ```tsx
   import { useVirtualizer } from '@tanstack/react-virtual';
   import { useRef } from 'react';

   interface Task { id: string; title: string; done: boolean; priority: string }

   function VirtualTaskList({ tasks }: { tasks: Task[] }) {
     const containerRef = useRef<HTMLDivElement>(null);

     const virtualizer = useVirtualizer({
       count: tasks.length,
       getScrollElement: () => containerRef.current,
       estimateSize: () => 56,
       overscan: 5, // renderizar 5 ítems extra fuera del viewport
     });

     return (
       <div
         ref={containerRef}
         className="h-full overflow-y-auto"
       >
         <div
           style={{ height: `${virtualizer.getTotalSize()}px` }}
           className="relative w-full"
         >
           {virtualizer.getVirtualItems().map(vItem => {
             const task = tasks[vItem.index];
             return (
               <div
                 key={task.id}
                 data-index={vItem.index}
                 ref={virtualizer.measureElement}
                 style={{ transform: `translateY(${vItem.start}px)` }}
                 className="absolute left-0 right-0 px-4 py-2"
               >
                 <TaskRow task={task} />
               </div>
             );
           })}
         </div>
       </div>
     );
   }
   ```

3. TaskRow como componente separado y memoizado:
   ```tsx
   const TaskRow = memo(function TaskRow({ task }: { task: Task }) {
     return (
       <div className="flex items-center gap-3 rounded-lg bg-surface-alt p-3">
         <input type="checkbox" checked={task.done} readOnly />
         <span className={task.done ? 'line-through text-text-muted' : ''}>{task.title}</span>
         <Badge>{task.priority}</Badge>
       </div>
     );
   });
   ```

4. Con filtros y búsqueda (virtualizar el array filtrado):
   ```tsx
   const filteredTasks = useMemo(
     () => tasks.filter(t => t.title.toLowerCase().includes(search.toLowerCase())),
     [tasks, search]
   );
   // Pasar filteredTasks al virtualizer (count = filteredTasks.length)
   ```

## Ejemplo
Lista de 5.000 tareas renderiza solo ~15 DOM nodes visibles en cualquier momento.

## Errores comunes
- **Contenedor sin `height`**: el virtualizador necesita un contenedor con altura definida y `overflow-y-auto`.
- **Keys usando el índice virtual**: usar `task.id` como key, no `vItem.index`.
- **No memoizar TaskRow**: sin `memo`, cada scroll re-renderiza todos los ítems visibles.
