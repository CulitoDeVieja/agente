# react-virtual-list

## Propósito
Renderizar listas con miles de ítems de forma eficiente usando virtualización en React.

## Cuándo usar
- Listas con más de ~200 elementos donde el rendimiento se degrada
- Tablas con scroll infinito o paginación virtual
- Logs, timelines o feeds en tiempo real

## Pasos

1. Instalar:
   ```bash
   npm install @tanstack/react-virtual
   ```

2. Implementar lista virtualizada:
   ```tsx
   import { useVirtualizer } from '@tanstack/react-virtual';
   import { useRef } from 'react';

   function VirtualList({ items }: { items: string[] }) {
     const parentRef = useRef<HTMLDivElement>(null);

     const virtualizer = useVirtualizer({
       count: items.length,
       getScrollElement: () => parentRef.current,
       estimateSize: () => 48, // altura estimada en px por ítem
     });

     return (
       <div ref={parentRef} style={{ height: '600px', overflowY: 'auto' }}>
         <div style={{ height: virtualizer.getTotalSize(), position: 'relative' }}>
           {virtualizer.getVirtualItems().map(vItem => (
             <div
               key={vItem.key}
               style={{
                 position: 'absolute',
                 top: vItem.start,
                 width: '100%',
                 height: vItem.size,
               }}
             >
               {items[vItem.index]}
             </div>
           ))}
         </div>
       </div>
     );
   }
   ```

3. Para ítems de altura variable:
   ```ts
   const virtualizer = useVirtualizer({
     count: items.length,
     getScrollElement: () => parentRef.current,
     estimateSize: () => 60,
     measureElement: (el) => el.getBoundingClientRect().height,
   });
   ```
   Agregar `ref={virtualizer.measureElement}` a cada ítem renderizado.

## Ejemplo
Lista de 10.000 tareas que scrollea fluidamente a 60fps.

## Errores comunes
- **Contenedor sin altura fija**: el contenedor padre necesita `height` y `overflow: auto`.
- **Keys duplicadas**: usar `vItem.key`, no el índice del array directamente.
- **Scroll jump al actualizar**: si los ítems cambian de tamaño, usar `measureElement`.
