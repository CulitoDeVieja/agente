# STATE — memoria global del sistema

**Última actualización:** 2026-04-19

## Agentes activos

| Rol | Ubicación | Última señal |
|---|---|---|
| orchestrator | local | ✅ activo 2026-04-19 |
| skills-curator | VPS-1 | ⏳ esperando primer ciclo |
| builder | VPS-2 | ✅ activo 2026-04-19 |
| auditor-ops | VPS-3 | ⏳ esperando primer ciclo |
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
1. En paralelo: architect + skills-curator (arrancan ya, no se bloquean entre sí).
2. Builder (cuando los 2 anteriores terminen).
3. Auditor-ops (cuando builder termine).

## Decisiones pendientes del owner

(ninguna)

## Reglas de escritura de este archivo

- Solo el **orchestrator** edita `STATE.md`.
- Se actualiza cuando: un agente reporta señal, se agrega proyecto, owner toma decisión.
- No es log histórico — es snapshot del presente. El log vive en `tareas/completado/`.
