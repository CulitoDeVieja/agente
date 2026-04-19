# react-hot-toast

## Propósito
Mostrar notificaciones toast en React de forma sencilla con react-hot-toast.

## Cuándo usar
- Confirmar acciones (guardado, copiado, eliminado)
- Mostrar errores no bloqueantes
- Feedback de operaciones async

## Pasos

1. Instalar:
   ```bash
   npm install react-hot-toast
   ```

2. Agregar el `Toaster` en el root (una sola vez):
   ```tsx
   import { Toaster } from 'react-hot-toast';

   function App() {
     return (
       <>
         <Router />
         <Toaster
           position="bottom-right"
           toastOptions={{
             duration: 3000,
             style: { background: '#1e1e2e', color: '#cdd6f4' },
           }}
         />
       </>
     );
   }
   ```

3. Disparar toasts desde cualquier lugar:
   ```ts
   import toast from 'react-hot-toast';

   toast('Mensaje neutral');
   toast.success('Guardado correctamente');
   toast.error('No se pudo conectar');
   toast.loading('Procesando...');

   // Toast con promesa (auto success/error):
   toast.promise(
     saveTask(data),
     {
       loading: 'Guardando...',
       success: 'Tarea guardada',
       error: (e) => `Error: ${e.message}`,
     }
   );
   ```

4. Toast personalizado:
   ```ts
   toast.custom((t) => (
     <div className={`toast ${t.visible ? 'show' : 'hide'}`}>
       ✅ Operación completada
     </div>
   ));
   ```

## Ejemplo
```ts
const handleDelete = async () => {
  await toast.promise(deleteItem(id), {
    loading: 'Eliminando...',
    success: 'Eliminado',
    error: 'No se pudo eliminar',
  });
};
```

## Errores comunes
- **Toasts duplicados**: no colocar `<Toaster>` en múltiples componentes.
- **Toast no desaparece**: `toast.loading` dura indefinido; siempre resolver con `toast.dismiss(id)`.
- **Z-index tapado**: ajustar con `containerStyle={{ zIndex: 9999 }}` en el Toaster.
