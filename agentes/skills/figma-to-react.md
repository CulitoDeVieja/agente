# figma-to-react

## Propósito
Convertir diseños Figma a componentes React + Tailwind CSS manteniendo fidelidad al diseño.

## Cuándo usar
- Implementar un diseño entregado por el equipo de diseño
- Asegurar que los valores de diseño (colores, espaciado, tipografía) sean exactos
- Crear componentes reutilizables desde mockups

## Pasos

1. Preparar el diseño en Figma:
   - Verificar que los colores usen variables/estilos (no valores hex sueltos)
   - Verificar que el texto use estilos de texto (no overrides manuales)
   - Usar Auto Layout (equivalente a flexbox) para facilitar la traducción

2. Inspeccionar valores en Figma Dev Mode:
   - Clic derecho → "Inspect" o panel derecho en modo Dev
   - Copiar: colores, padding, gap, border-radius, font-size, font-weight

3. Traducir Auto Layout a Tailwind:
   ```
   Figma Auto Layout (horizontal, gap 16, padding 12 24)
   → className="flex flex-row gap-4 py-3 px-6"
   ```

4. Crear el componente:
   ```tsx
   // Ejemplo: Card de tarea desde Figma
   interface TaskCardProps {
     title: string;
     priority: 'low' | 'medium' | 'high';
     dueDate: string;
   }

   const priorityColor = {
     low: 'bg-green-500/20 text-green-400',
     medium: 'bg-yellow-500/20 text-yellow-400',
     high: 'bg-red-500/20 text-red-400',
   };

   export function TaskCard({ title, priority, dueDate }: TaskCardProps) {
     return (
       <div className="flex items-center gap-4 rounded-xl bg-surface p-4 shadow-sm">
         <div className="flex-1">
           <p className="text-sm font-medium text-text-primary">{title}</p>
           <p className="text-xs text-text-muted">{dueDate}</p>
         </div>
         <span className={`rounded-full px-2 py-1 text-xs font-semibold ${priorityColor[priority]}`}>
           {priority}
         </span>
       </div>
     );
   }
   ```

5. Mapear variables Figma → tokens Tailwind:
   - Color `Surface/Default` → `bg-surface`
   - Color `Text/Muted` → `text-text-muted`
   - Spacing `4` → `p-4` (16px si base-4)

## Ejemplo
Un frame Figma de 360×64px con Auto Layout horizontal se convierte en un `<div className="flex h-16 w-full ...">`.

## Errores comunes
- **Valores px en lugar de tokens**: hardcodear `style={{ padding: '14px' }}` en lugar de clases Tailwind rompe el sistema de diseño.
- **Ignorar estados (hover, focus, disabled)**: Figma puede tener componentes con variantes; implementarlas todas.
- **Fuentes no cargadas**: verificar que la fuente de Figma esté importada en el proyecto.
