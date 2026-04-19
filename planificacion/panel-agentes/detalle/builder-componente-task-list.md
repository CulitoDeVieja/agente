# Plan: componente-task-list

## Objetivo
Componente reutilizable `TaskList.tsx` para renderizar lista de tareas.

## Props
```ts
type TaskListProps = {
  tasks: Task[];
  showBlocked?: boolean;
  maxItems?: number;
}
```

## Pasos
1. Renderizar cada tarea como fila: título + prioridad + estado
2. Si `showBlocked`: mostrar 🔒 y dependencias faltantes
3. Truncar a `maxItems` (default 10 para completadas)

## Tests planeados
- [ ] Renderiza lista vacía sin error
- [ ] Trunca a maxItems correctamente
