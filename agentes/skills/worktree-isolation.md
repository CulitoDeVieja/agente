# worktree-isolation

## Propósito
Que 2 agentes trabajen en el mismo repo sin chocarse.

## Cuándo usar
Siempre que un Builder o Auditor vaya a modificar archivos de un proyecto compartido.

## Concepto
`git worktree` crea una copia-de-trabajo del repo en otra carpeta, apuntando al mismo `.git`. Cada agente tiene su worktree con su propia HEAD, pero todos pushean al mismo origin.

## Pasos
1. Primera vez — clonar repo base:
   ```
   git clone <url> /opt/proyectos/<proyecto>
   cd /opt/proyectos/<proyecto>
   ```
2. Cada agente crea su worktree la primera vez:
   ```
   git worktree add /opt/worktrees/<proyecto>-builder builder/main
   ```
3. Trabajar en `/opt/worktrees/<proyecto>-builder`.
4. Commit + `git pull --rebase origin main` + `git push`.

## Reglas
- Cada agente tiene **una branch propia** (ej: `builder/main`, `auditor/main`). Nunca editan en `main` directo.
- Orchestrator mergea las branches de agentes a `main` después de validar.
- Si 2 agentes piden el mismo archivo → orchestrator encola al segundo.

## Limpieza
- Worktrees viejos se eliminan con `git worktree remove <path>`.
- Revisar cada semana con `git worktree list`.

## Errores comunes
- "fatal: '/opt/worktrees/...' is not a working tree" → worktree corrupto. Eliminar y recrear.
- Agente hace push a `main` directo por error → rollback inmediato, reabrir como PR desde su branch.
