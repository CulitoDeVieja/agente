# MODO MASTER

Cuando el owner pide "modo master", el orchestrator coordina a todos los agentes en una sola tarea grande.

## Qué hace el orchestrator

1. **Descompone** la tarea grande en sub-tareas por rol.
2. **Encadena** las sub-tareas con dependencias explícitas (`## Depende de: <archivo>`).
3. **Crea** los archivos en `tareas/pendiente/` para cada rol simultáneamente.
4. **Monitorea** `tareas/completado/` cada ciclo. Cuando una dependencia se cumple, libera la siguiente (si estaba bloqueada, se marca como disponible).
5. **Actualiza** `STATE.md` sección "Modo Master activo" con:
   - Tarea madre
   - Sub-tareas por rol, con estado (pendiente / en-curso / completado).
   - Quién está esperando a quién.

## Regla de dependencias

En cada sub-tarea, bloque `## Depende de:` lista archivos que deben estar en `completado/` antes de que el agente la tome.

Si el agente encuentra que su dependencia no está cumplida → deja la tarea en `pendiente/` y sigue.

## Cuándo termina MODO MASTER

Cuando todas las sub-tareas están en `completado/`. El orchestrator consolida y reporta al owner.
