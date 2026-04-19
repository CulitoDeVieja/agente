# anti-choques

## Propósito
Que los 4 agentes trabajen en el mismo repo sin romper commits del otro.

## Cuándo usar
Siempre. Skill base de todos los agentes en loop.

## Regla 1 — Namespace por rol (dura)
Cada agente solo escribe archivos que empiezan con su rol:
- orchestrator → `orchestrator-*`, `STATE.md`, `PLAN-MASTER-*.md`, `MODO-MASTER.md`
- skills-curator → `skills-curator-*`, `agentes/skills/*.md`
- builder → `builder-*`
- auditor-ops → `auditor-ops-*`, `ops-*`

**Nunca** un agente edita un archivo con prefijo de otro.

## Regla 2 — Archivos compartidos (editar con lock)
Solo 3 archivos son editados por más de un agente:
- `agentes/skills/INDEX.md` (skills-curator + orchestrator)
- `STATE.md` (solo orchestrator — es de un solo rol, sin lock)
- `tareas/*/` (mueven archivos, no los editan)

Para `INDEX.md`:
1. Antes de editar: `git pull --rebase` inmediato.
2. Abrir, hacer el cambio mínimo, guardar, commitear, `git pull --rebase`, `git push`.
3. Si push falla con "non-fast-forward" → rebase y reintentar una vez más.
4. Si falla 3 veces → skipear esta ronda, reintentar próximo ciclo.

## Regla 3 — Retry con backoff en push
```bash
for intento in 1 2 3; do
  git pull --rebase && git push && break
  sleep $((intento * 5))
done
```
Nunca `git push --force`.

## Regla 4 — Mover, no reescribir
Una tarea que cambia de estado (`pendiente → en-curso → completado`) se **mueve** con `git mv`, no se borra y recrea. El historial git detecta el rename y evita conflictos.

## Regla 5 — Un commit por acción lógica
Un agente no acumula 10 cambios en un commit. Un push por acción:
- Tomar tarea → 1 commit (mv a en-curso)
- Escribir archivo → 1 commit
- Completar → 1 commit (mv a completado + log)

Así el rebase de otro agente es rápido.

## Errores comunes
- Editar un archivo de otro rol "por comodidad" → crea conflict trasparente en el próximo push del dueño.
- Batch commits grandes → rebase doloroso.
- Saltar `git pull --rebase` antes de push → rechazo garantizado.
- `git push --force` → destruye trabajo de otro agente. **Prohibido sin aprobación del owner.**

## Escalamiento
Si 3 ciclos seguidos fallan por conflict de merge → crear tarea `orchestrator-*` con el archivo problemático y dejar de tocarlo hasta que Orchestrator consolide.
