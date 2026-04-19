# empty-state-copy

## Propósito
Redactar estados vacíos (empty states) que orienten al usuario a la siguiente acción.

## Cuándo usar
- Lista de tareas vacía (primer uso o sin resultados)
- Búsqueda sin resultados
- Sección sin contenido aún configurado

## Pasos

### Estructura recomendada

```
[Ícono o ilustración]
[Título: qué hay aquí normalmente]
[Subtítulo: por qué está vacío + qué hacer]
[CTA: botón de acción principal]
```

### Tipos de empty state

1. **Primer uso** (onboarding):
   ```
   ✅ Todavía no tenés tareas
   Creá tu primera tarea para empezar a organizar tu trabajo.
   [Crear tarea]
   ```

2. **Sin resultados de búsqueda**:
   ```
   🔍 Sin resultados para "diseño mobile"
   Probá con otras palabras o revisá el filtro de prioridad.
   [Limpiar filtros]
   ```

3. **Filtros activos sin resultados**:
   ```
   📋 No hay tareas con prioridad alta
   Cambiá el filtro para ver otras tareas.
   [Ver todas las tareas]
   ```

4. **Sección en construcción**:
   ```
   📊 Los reportes estarán disponibles pronto
   Mientras tanto, podés ver el resumen en el dashboard.
   [Ir al dashboard]
   ```

### Componente en React

```tsx
interface EmptyStateProps {
  icon?: React.ReactNode;
  title: string;
  description?: string;
  action?: { label: string; onClick: () => void };
}

function EmptyState({ icon, title, description, action }: EmptyStateProps) {
  return (
    <div className="flex flex-col items-center justify-center gap-3 py-16 text-center">
      {icon && <div className="text-4xl text-text-muted">{icon}</div>}
      <p className="text-base font-semibold text-text-primary">{title}</p>
      {description && <p className="max-w-sm text-sm text-text-muted">{description}</p>}
      {action && (
        <button onClick={action.onClick} className="btn-primary mt-2">
          {action.label}
        </button>
      )}
    </div>
  );
}
```

### Tono recomendado
- Neutro y alentador, no apologético ("Lo sentimos, no hay nada")
- Orientado a la acción
- Claro sobre qué puede hacer el usuario

## Ejemplo
```tsx
<EmptyState
  icon="📋"
  title="Sin tareas pendientes"
  description="Todo al día. Creá una nueva tarea cuando la necesites."
  action={{ label: "Nueva tarea", onClick: openCreateModal }}
/>
```

## Errores comunes
- **Empty state genérico**: "No hay datos" no orienta al usuario; ser específico sobre qué falta.
- **Sin CTA**: un empty state sin acción es un callejón sin salida.
- **Texto muy largo**: el empty state debe ser escaneable; máximo 2 líneas de descripción.
