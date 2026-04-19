# Plan: componente-agent-detail

## Objetivo
Vista detalle `/agent/:role` con tareas del agente.

## Secciones
1. Header: rol + estado + botón volver
2. Pendientes: lista con prioridad + 🔒 si bloqueada
3. En curso: tarea activa con log parcial
4. Completadas: últimas 10 con fecha

## Pasos
1. Leer `role` de params
2. Filtrar `tasks` del context por rol
3. Agrupar por estado
4. Marcar bloqueadas: `dependeDe` contiene archivos no en completado

## Tests planeados
- [ ] Filtra tareas por rol correctamente
- [ ] Tarea bloqueada muestra 🔒
- [ ] Máximo 10 completadas visibles
