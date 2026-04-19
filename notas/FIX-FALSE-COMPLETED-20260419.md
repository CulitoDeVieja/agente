# Fix: 204 tareas false-completed revertidas a `pendiente/`

**Fecha:** 2026-04-19
**Ejecutado por:** `lupa` (pincel detallista del proyecto Ragnarok/DECK)
**Pedido por:** Antonio — *"los agentes dicen que no tienen tareas pero STATE.md dice que tienen 100+ pendientes"*

## Diagnóstico

Cuando los agentes de VPS-1 (skills-curator) y VPS-3 (auditor-ops) ejecutan su `ciclo.sh`, hacen:

```bash
ls tareas/pendiente/ | grep ^$AGENT_ROLE-
```

Y reciben **0 resultados** → reportan "sin tareas".

Mientras `STATE.md` dice "skills-curator: 24 pendientes" y "auditor-ops: 100 pendientes".

**Causa:** `tareas/completado/` tenía **204 archivos con `## Log del agente (vacío hasta completar)`**. Esos archivos fueron movidos a `completado/` **sin que el agente los ejecutara realmente**. Mecanismos detectados:

1. **By-pass manual del owner** (Antonio): commit `ORCH: completa 002 — Fase A aprobada (by-pass deps por owner)`
2. **Agentes tocando tareas ajenas**: `psique` completó tareas de `auditor-ops`
3. **STATE.md escrito a ojo**, no regenerado del filesystem

## Lo que hice (fix)

Script que escaneó los 378 archivos en `completado/` y detectó false-completed por:
- Log vacío, o
- Log == `(vacío hasta completar)`, o
- Log < 20 chars (trivial)

**Resultado del escaneo:**

| Rol | False-completed | Real-completed |
|---|---|---|
| skills-curator | **100** | 1 |
| builder | **99** | 2 |
| orchestrator | 4 | 0 |
| auditor-ops | 1 | 99 |
| architect | 0 | 72 |
| **Total** | **204** | **174** |

**Acción:** `git mv` de los 204 archivos `completado/ → pendiente/`.

## Estado real después del fix

| Rol | Pendiente | En-curso | Completado REAL |
|---|---|---|---|
| architect | 0 | 0 | 72 |
| auditor-ops | 1 | 0 | 99 |
| builder | 99 | 0 | 2 |
| orchestrator | 4 | 1 | 0 |
| skills-curator | 100 | 0 | 1 |

**Total pendiente después del fix: 204 tareas** (era 1 antes).

## Para Zeus (orchestrator)

Acciones recomendadas en próximo ciclo:

1. **Actualizar STATE.md** con los números reales de arriba (hoy está desincronizado)
2. **Regla dura nueva:** al completar tarea, `ciclo.sh` debe verificar que el log NO esté vacío antes de `git mv` a `completado/`. Agregar guard:
   ```bash
   if grep -q "(vacío hasta completar)" "$TASK_FILE"; then
     echo "ERROR: log vacío, no se marca done"
     exit 1
   fi
   ```
3. **Regla dura nueva:** un agente sólo puede mover sus propios archivos (`<rol>-NNN-*.md`). Quien tiene `AGENT_ROLE=X` no puede `git mv` un `<rol_ajeno>-NNN-*.md`.
4. **By-pass del owner debe dejar log explícito** en el archivo antes del move. Nunca mover sin dejar evidencia.
5. **Herramienta de salud del sistema:** script `tools/health.sh` que corra los checks y reporte si hay false-completed, desalineación con STATE.md, etc. Correr cada ciclo.

## Cómo se detectó

`lupa` hizo un `audit de salud` ad-hoc desde sesión web, detectó la inconsistencia entre STATE.md y filesystem real, validó el patrón de log vacío, reportó, y fixeó con autorización directa de Antonio.

## Lista completa de archivos movidos

Los 204 archivos están en `/tmp/false_completed.txt` del sistema donde corrió lupa. Post-mortem: la lista se puede regenerar con:

```bash
git log --oneline --diff-filter=R --follow tareas/pendiente/ | head -300
```

o revisando el diff de este PR.

---

🔍 **lupa** — fix aplicado, status reportado, bola al orchestrator
