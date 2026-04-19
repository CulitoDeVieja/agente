# Tarea: Unificar output Fase B en loop constante

**Rol:** orchestrator
**Prioridad:** alta (continua)
**Creada:** 2026-04-19
**Modo master:** panel-agentes — Fase B ejecución

## Qué hacer (continuo, no termina hasta que Fase B cierra)

En cada ciclo durante Fase B:
1. `git pull --rebase` ambos repos (agente + panel-agentes).
2. Detectar tareas recién completadas (comparar timestamp último ciclo).
3. Leer el log de cada una: `Tests: ✅` o `❌`.
4. Si ✅ → sumar al tracker de progreso en STATE.md.
5. Si ❌ → verificar que haya tarea `*-fix-*` creada; si no, crearla.
6. Cada 5 tareas completadas → escribir `notas/orchestrator/union-v0.1-<N>.md` consolidando qué está integrado.

## Acceptance criteria
- [ ] STATE.md refleja progreso Fase B en tiempo real.
- [ ] Cero errores sin tarea de fix asignada.
- [ ] Al menos 3 notas `union-v0.1-*.md` escritas cuando haya ≥15 completadas.

## Depende de:
(ninguna — arranca cuando Fase B arranca)

## Habilita:
- Cierre de modo master cuando builder-002 + auditor-ops-002 estén en completado.

---

## Log del agente
(continuo, se actualiza en cada ciclo)
