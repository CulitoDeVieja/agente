# screen-reader-aria

## Propósito
Implementar ARIA roles y labels correctamente en React para que los lectores de pantalla funcionen.

## Cuándo usar
- Componentes interactivos sin semántica nativa (divs, spans como botones)
- Mensajes de estado dinámicos (loading, errores, success)
- Iconos sin texto visible

## Pasos

1. Labels para elementos sin texto visible:
   ```tsx
   // Botón con solo ícono:
   <button aria-label="Eliminar tarea">
     <TrashIcon aria-hidden="true" />
   </button>

   // Input sin label visible:
   <input type="search" aria-label="Buscar tareas" placeholder="Buscar..." />

   // Relacionar label con input:
   <label htmlFor="task-title">Título</label>
   <input id="task-title" type="text" />
   ```

2. Roles para elementos custom:
   ```tsx
   <div role="alert" aria-live="polite">
     {errorMessage}
   </div>

   <div role="status" aria-live="polite" aria-atomic="true">
     {statusMessage}
   </div>
   ```

3. Live regions para actualizaciones dinámicas:
   ```tsx
   // Anunciar cambios sin mover el foco:
   function Announcer({ message }: { message: string }) {
     return (
       <div
         role="status"
         aria-live="polite"
         aria-atomic="true"
         className="sr-only"
       >
         {message}
       </div>
     );
   }

   // Uso: <Announcer message="3 tareas encontradas" />
   ```

4. Descripción extendida con `aria-describedby`:
   ```tsx
   <button aria-describedby="delete-warning" onClick={deleteAll}>
     Eliminar todo
   </button>
   <p id="delete-warning" className="sr-only">
     Esta acción eliminará permanentemente todas las tareas.
   </p>
   ```

5. Esconder elementos decorativos:
   ```tsx
   <img src="/decoration.svg" alt="" aria-hidden="true" />
   <span aria-hidden="true">✓</span> Guardado
   ```

6. Estado de componentes:
   ```tsx
   <button aria-pressed={isActive} onClick={toggle}>
     Modo oscuro
   </button>

   <div role="tab" aria-selected={isSelected} tabIndex={isSelected ? 0 : -1}>
     Proyectos
   </div>
   ```

## Ejemplo
Un spinner de loading: `<div role="status" aria-label="Cargando tareas..." className="sr-only">` anuncia al lector de pantalla sin mostrarse visualmente.

## Errores comunes
- **`aria-label` en elementos con texto**: si el elemento ya tiene texto visible, no se necesita `aria-label`.
- **Roles incorrectos**: `role="button"` en un `<a>` es confuso; usar el elemento semántico correcto.
- **`aria-live` demasiado agresivo**: `aria-live="assertive"` interrumpe al usuario; usar `polite` por defecto.
