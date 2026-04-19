# accessibility-keyboard

## Propósito
Implementar navegación por teclado accesible en componentes React para usuarios que no usan mouse.

## Cuándo usar
- Menús desplegables, modales, tooltips y componentes interactivos custom
- Cumplir WCAG 2.1 criterio 2.1 (Keyboard Accessible)
- Garantizar que la app sea operable solo con teclado

## Pasos

1. Orden de foco con `tabIndex`:
   ```tsx
   // Correcto: elementos interactivos son focusables por defecto
   <button onClick={save}>Guardar</button>
   <a href="/home">Inicio</a>

   // Custom element interactivo:
   <div
     role="button"
     tabIndex={0}
     onClick={handleClick}
     onKeyDown={(e) => { if (e.key === 'Enter' || e.key === ' ') handleClick(); }}
   >
     Acción custom
   </div>
   ```

2. Trampa de foco en modales:
   ```tsx
   import { useEffect, useRef } from 'react';

   function Modal({ onClose }: { onClose: () => void }) {
     const firstFocusRef = useRef<HTMLButtonElement>(null);

     useEffect(() => {
       firstFocusRef.current?.focus(); // mover foco al abrir
     }, []);

     const handleKeyDown = (e: React.KeyboardEvent) => {
       if (e.key === 'Escape') onClose();
     };

     return (
       <div role="dialog" aria-modal="true" onKeyDown={handleKeyDown}>
         <button ref={firstFocusRef} onClick={onClose}>Cerrar</button>
         {/* contenido del modal */}
       </div>
     );
   }
   ```

3. Navegación por lista con flechas:
   ```tsx
   function Menu({ items }: { items: string[] }) {
     const [focused, setFocused] = useState(0);

     const handleKeyDown = (e: React.KeyboardEvent) => {
       if (e.key === 'ArrowDown') setFocused(f => Math.min(f + 1, items.length - 1));
       if (e.key === 'ArrowUp') setFocused(f => Math.max(f - 1, 0));
       if (e.key === 'Enter') selectItem(items[focused]);
     };

     return (
       <ul role="listbox" onKeyDown={handleKeyDown}>
         {items.map((item, i) => (
           <li key={item} role="option" aria-selected={i === focused}
               tabIndex={i === focused ? 0 : -1}>
             {item}
           </li>
         ))}
       </ul>
     );
   }
   ```

4. Skip link para saltar navegación:
   ```tsx
   <a href="#main-content" className="sr-only focus:not-sr-only focus:fixed focus:top-4 focus:left-4 z-50 bg-brand-500 text-white px-4 py-2 rounded">
     Saltar al contenido principal
   </a>
   ```

## Ejemplo
Modal de confirmación: al abrir, el foco va al botón "Cancelar"; `Escape` cierra; `Tab` cicla dentro del modal.

## Errores comunes
- **`div` clickeable sin `tabIndex={0}`**: los divs no son focusables; añadir `tabIndex` y `onKeyDown`.
- **Outline de foco eliminado**: `outline: none` sin reemplazo visual deja al usuario sin indicador de foco.
- **Foco que escapa del modal**: implementar focus trap para modales y drawers.
