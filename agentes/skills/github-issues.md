# github-issues

## Propósito
Tomar, trabajar y cerrar tareas asignadas vía GitHub issues.

## Cuándo usar
Cada inicio de ciclo de un agente en loop.

## Requisitos
- `gh` (GitHub CLI) instalado y autenticado en el VPS.
- Rol del agente definido como variable de entorno: `export AGENT_ROLE=builder` (o el que sea).

## Pasos al iniciar ciclo
1. Listar issues abiertos asignados al rol:
   ```
   gh issue list --repo <owner>/orchestrator --assignee "$AGENT_ROLE" --state open --json number,title,labels
   ```
2. Tomar el primero. Marcarlo `in-progress`:
   ```
   gh issue edit <num> --add-label in-progress
   ```
3. Leer el body completo:
   ```
   gh issue view <num> --repo <owner>/orchestrator
   ```
4. Ejecutar la tarea.
5. Cerrar con comentario resultado:
   ```
   gh issue close <num> --comment "Hecho. Commit abc1234. Tests ✓"
   ```

## Cuando pide ayuda
Si necesita skill nueva o no entiende la spec:
```
gh issue create --repo <owner>/orchestrator \
  --title "skill-request: <qué falta>" \
  --label skill-request \
  --assignee skills-curator
```

## Errores comunes
- `gh auth status` falla → re-loguear con `gh auth login`.
- Issue sin label `type:*` → tratarlo como `type:build` por defecto y avisar al orchestrator.
- Issue con múltiples asignados → rechazar, pedir clarificación al orchestrator.
