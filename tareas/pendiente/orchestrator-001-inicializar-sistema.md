# Tarea: Inicializar sistema multi-agente

**Rol:** orchestrator
**Prioridad:** alta
**Creada:** 2026-04-19
**Proyecto:** agente (meta-tarea)

## Qué hacer
Validar que los 4 VPS están configurados según `SETUP-VPS.md`:
- Local: orchestrator
- VPS-1: skills-curator
- VPS-2: builder
- VPS-3: auditor-ops

## Acceptance criteria
- [ ] Los 4 pueden hacer `git pull` y `git push` al repo `agente`.
- [ ] Cada uno tiene `$AGENT_ROLE` correcto.
- [ ] `claude --version` funciona en los 4.
- [ ] STATE.md actualizado con señal "activo" en los 4 roles con loop.

## Skills probables
- skills/git-workflow.md
- skills/tareas-markdown.md

## Dependencias
Owner completó SETUP-VPS.md en las 4 máquinas.

---

## Log del agente
(vacío hasta completar)
