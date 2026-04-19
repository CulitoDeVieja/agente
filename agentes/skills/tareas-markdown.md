# tareas-markdown

## Propósito
Tomar, trabajar y completar tareas desde la carpeta `tareas/` del repo `agente`. Reemplaza a GitHub issues.

## Cuándo usar
Cada inicio de ciclo de un agente en loop.

## Pasos al iniciar ciclo

1. Pull del repo agente:
   ```
   cd /opt/agente/agente && git pull --rebase
   ```

2. Verificar si ya estoy trabajando en algo (me quedé a medias en el ciclo anterior):
   ```
   ls tareas/en-curso/ | grep "^$AGENT_ROLE-"
   ```
   Si hay uno → seguir con esa tarea. Si no → buscar nueva.

3. Listar tareas pendientes para mi rol:
   ```
   ls tareas/pendiente/ | grep "^$AGENT_ROLE-" | head -1
   ```

4. Tomar la primera:
   ```
   git mv tareas/pendiente/<archivo>.md tareas/en-curso/
   git commit -m "<rol>: toma tarea <id>"
   git pull --rebase && git push
   ```

5. Leer el archivo completo de la tarea (qué hacer, AC, skills probables).

6. Cargar skills lazy según lo que sugiere el archivo.

7. Ejecutar la tarea en el proyecto correspondiente.

8. Al terminar, agregar log al final del archivo de tarea:
   ```
   ## Log del agente
   - Commit: abc1234
   - Tests: ✓ (3 pasan)
   - AC cumplidos: 1, 2
   - Notas: ...
   ```

9. Mover a completado:
   ```
   git mv tareas/en-curso/<archivo>.md tareas/completado/
   git commit -m "<rol>: completa tarea <id>"
   git pull --rebase && git push
   ```

## Si la cola está vacía
Reportar "cola vacía" y salir. No generar trabajo sin tarea asignada.

## Si falla 3 veces
1. Crear archivo nuevo `tareas/pendiente/skills-curator-skill-para-<tarea>.md` describiendo qué falta.
2. Mover la tarea actual de `en-curso/` de vuelta a `pendiente/` con sufijo `-bloqueada`.
3. Salir.

## Errores comunes
- Dos agentes toman la misma tarea → `git pull --rebase` falla al mover. Reintentá (el que llega primero gana, el otro busca otra).
- `git mv` sin commit intermedio → el archivo queda en limbo. Siempre commitear después de mv.
