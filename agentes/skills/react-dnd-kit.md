# react-dnd-kit

## Propósito
Implementar drag and drop accesible y flexible en React usando @dnd-kit/core.

## Cuándo usar
- Reordenar listas (tareas, columnas Kanban, items)
- Mover elementos entre contenedores
- Necesidad de accesibilidad teclado en DnD

## Pasos

1. Instalar:
   ```bash
   npm install @dnd-kit/core @dnd-kit/sortable @dnd-kit/utilities
   ```

2. Implementar lista sorteable:
   ```tsx
   import {
     DndContext, closestCenter, PointerSensor, useSensor, useSensors,
   } from '@dnd-kit/core';
   import {
     SortableContext, verticalListSortingStrategy,
     useSortable, arrayMove,
   } from '@dnd-kit/sortable';
   import { CSS } from '@dnd-kit/utilities';

   function SortableItem({ id, label }: { id: string; label: string }) {
     const { attributes, listeners, setNodeRef, transform, transition } =
       useSortable({ id });
     const style = { transform: CSS.Transform.toString(transform), transition };
     return (
       <div ref={setNodeRef} style={style} {...attributes} {...listeners}>
         {label}
       </div>
     );
   }

   function SortableList({ items, setItems }) {
     const sensors = useSensors(useSensor(PointerSensor));

     return (
       <DndContext
         sensors={sensors}
         collisionDetection={closestCenter}
         onDragEnd={({ active, over }) => {
           if (over && active.id !== over.id) {
             setItems(prev => arrayMove(
               prev,
               prev.findIndex(i => i.id === active.id),
               prev.findIndex(i => i.id === over.id),
             ));
           }
         }}
       >
         <SortableContext items={items} strategy={verticalListSortingStrategy}>
           {items.map(i => <SortableItem key={i.id} id={i.id} label={i.label} />)}
         </SortableContext>
       </DndContext>
     );
   }
   ```

## Ejemplo
Tablero Kanban donde las tarjetas se arrastran entre columnas "Por hacer" y "En progreso".

## Errores comunes
- **IDs no únicos**: todos los ítems deben tener `id` único como string.
- **Conflicto con click**: usar `activationConstraint: { distance: 8 }` en el sensor para distinguir.
- **Sin accesibilidad**: agregar `KeyboardSensor` junto a `PointerSensor` para navegación por teclado.
