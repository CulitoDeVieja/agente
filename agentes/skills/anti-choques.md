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

## Regla 5 — Un commit + push por tarea (DURA)
**Cada tarea terminada se sube inmediatamente. No se acumula.**

Secuencia por tarea (3 pushes):
1. Tomar → `git mv pendiente/X en-curso/` → commit → **push**
2. Ejecutar → escribir archivos de output → commit → **push**
3. Completar → `git mv en-curso/X completado/` + log → commit → **push**

**Prohibido:** procesar 5 tareas y hacer 1 solo push al final. Rompe la visibilidad del owner y genera rebases pesados para los demás.

Si vas a hacer 50 tareas en un ciclo → 150 commits mínimo. El cron de 1 min debe rendirse si está por terminarse y no llegó a la tarea 50 → completa lo que tenés, push, y la siguiente ronda la toma el próximo ciclo.

## Errores comunes
- Editar un archivo de otro rol "por comodidad" → crea conflict trasparente en el próximo push del dueño.
- Batch commits grandes → rebase doloroso.
- Saltar `git pull --rebase` antes de push → rechazo garantizado.
- `git push --force` → destruye trabajo de otro agente. **Prohibido sin aprobación del owner.**

## Escalamiento
Si 3 ciclos seguidos fallan por conflict de merge → crear tarea `orchestrator-*` con el archivo problemático y dejar de tocarlo hasta que Orchestrator consolide.
