# confirm-destructive-action

## Propósito
Implementar el patrón de confirmación antes de ejecutar acciones destructivas (eliminar, resetear, borrar).

## Cuándo usar
- Eliminar ítems (tareas, proyectos, cuentas)
- Resetear configuración a valores por defecto
- Acciones irreversibles que afectan datos importantes

## Pasos

1. Decidir el nivel de confirmación según el riesgo:
   - **Bajo riesgo** (deshacer posible): eliminar un ítem con undo → Toast + Deshacer
   - **Riesgo medio** (afecta pocos datos): modal de confirmación simple
   - **Riesgo alto** (datos críticos, irreversible): confirmación con texto escrito

2. Modal de confirmación simple:
   ```tsx
   interface ConfirmDialogProps {
     open: boolean;
     title: string;
     description: string;
     confirmLabel?: string;
     onConfirm: () => void;
     onCancel: () => void;
   }

   function ConfirmDialog({ open, title, description, confirmLabel = 'Eliminar',
     onConfirm, onCancel }: ConfirmDialogProps) {
     if (!open) return null;
     return (
       <div role="dialog" aria-modal="true" aria-labelledby="confirm-title"
            className="fixed inset-0 z-50 flex items-center justify-center bg-black/50">
         <div className="w-full max-w-sm rounded-xl bg-surface p-6 shadow-xl">
           <h2 id="confirm-title" className="text-base font-semibold">{title}</h2>
           <p className="mt-2 text-sm text-text-muted">{description}</p>
           <div className="mt-6 flex justify-end gap-3">
             <button onClick={onCancel} className="btn-ghost">Cancelar</button>
             <button onClick={onConfirm} className="btn-danger">{confirmLabel}</button>
           </div>
         </div>
       </div>
     );
   }
   ```

3. Confirmación con texto escrito (alta criticidad):
   ```tsx
   function DeleteAccountConfirm({ onConfirm }) {
     const [input, setInput] = useState('');
     const REQUIRED = 'ELIMINAR';

     return (
       <div>
         <p>Escribí <strong>{REQUIRED}</strong> para confirmar:</p>
         <input value={input} onChange={e => setInput(e.target.value)} />
         <button
           disabled={input !== REQUIRED}
           onClick={onConfirm}
           className="btn-danger disabled:opacity-40"
         >
           Eliminar cuenta permanentemente
         </button>
       </div>
     );
   }
   ```

4. Toast con Deshacer (acciones de bajo riesgo):
   ```ts
   const handleDelete = () => {
     const backup = task;
     deleteTask(task.id);
     toast.custom((t) => (
       <div className="flex gap-3">
         <span>Tarea eliminada</span>
         <button onClick={() => { restoreTask(backup); toast.dismiss(t.id); }}>
           Deshacer
         </button>
       </div>
     ), { duration: 5000 });
   };
   ```

## Ejemplo
Eliminar proyecto: modal con título, descripción del impacto ("Se eliminarán X tareas") y botón rojo "Eliminar proyecto".

## Errores comunes
- **Confirmar acciones reversibles**: no pedir confirmación para acciones con undo; interrumpe el flujo.
- **Botón destructivo como acción principal**: el botón de confirm destructivo debe ser secundario visualmente (a la derecha, rojo); "Cancelar" como salida fácil.
- **Sin describir el impacto**: "¿Estás seguro?" sin contexto no ayuda; describir qué se perderá.
