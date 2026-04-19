# STATE — memoria global del sistema

**Última actualización:** 2026-04-19

## Agentes activos

| Rol | Ubicación | Última señal |
|---|---|---|
| orchestrator | local | ✅ activo 2026-04-19 |
| skills-curator | VPS-1 | ✅ activo 2026-04-19 (sincronizado modo master) |
| builder | VPS-2 | ✅ activo 2026-04-19 (sincronizado modo master) |
| auditor-ops | VPS-3 | ✅ activo 2026-04-19 (sincronizado modo master) |
| scout | on-demand | — |
| architect | on-demand | — |

## Proyectos activos

| Proyecto | Repo | Agentes que lo tocan |
|---|---|---|
| panel-agentes | panel-agentes (a crear) | architect → builder → auditor |

## MODO MASTER activo

**Tarea madre:** panel-agentes (app Tauri Windows)

| Sub-tarea | Rol | Estado | Depende de |
|---|---|---|---|
| architect-001-panel-agentes-spec | architect | pendiente | — |
| skills-curator-002-skill-tauri-architecture | skills-curator | pendiente | — |
| builder-002-implementar-panel-agentes | builder | bloqueada | architect-001 + skills-curator-002 |
| auditor-ops-002-audit-panel-agentes | auditor-ops | bloqueada | builder-002 |

**Orden de ejecución esperado:**

**Fase A — Planificación** (obligatoria antes de programar, `planificacion/panel-agentes/`):
1. architect escribe `01-arquitectura.md`
2. skills-curator escribe `02-skills.md` (paralelo con architect)
3. builder escribe `03-implementacion.md` (espera 01+02)
4. auditor-ops escribe `04-auditoria.md` (espera 03)
5. orchestrator aprueba `revision.md`

**Fase B — Ejecución** (solo tras Fase A aprobada):
6. builder ejecuta `builder-002-implementar-panel-agentes`
7. auditor-ops ejecuta `auditor-ops-002-audit-panel-agentes`

## Decisiones pendientes del owner

(ninguna)

## Reglas de escritura de este archivo

- Solo el **orchestrator** edita `STATE.md`.
- Se actualiza cuando: un agente reporta señal, se agrega proyecto, owner toma decisión.
- No es log histórico — es snapshot del presente. El log vive en `tareas/completado/`.
