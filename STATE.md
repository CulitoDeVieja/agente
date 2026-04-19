# STATE — memoria global del sistema

**Última actualización:** 2026-04-19 · `lupa` revirtió 204 false-completed a `pendiente/`

## 🔴 Incidente 2026-04-19 — 204 tareas false-completed

Ver `notas/FIX-FALSE-COMPLETED-20260419.md` + `tareas/pendiente/orchestrator-100-regenerar-estado-real.md`.

Resumen: el 54% de las tareas en `completado/` tenían `## Log del agente (vacío)`. Fueron movidas por by-pass manual del owner, cross-role picking (psique tocó auditor-ops), y el STATE.md se escribía a ojo en vez de regenerar del filesystem.

**Status:** revertidas por lupa a `pendiente/`. Los agentes VPS ahora sí tienen tareas reales. Orchestrator debe ejecutar `orchestrator-100-regenerar-estado-real.md` para regenerar guards y docs.

## Agentes activos (números REALES del filesystem)

| Rol | Ubicación | Pendiente | En-curso | Completado REAL |
|---|---|---|---|---|
| orchestrator | local | 4 | 1 | 0 |
| skills-curator | VPS-1 | **100** | 0 | 1 |
| builder | VPS-2 | **99** | 0 | 2 |
| auditor-ops | VPS-3 | 1 | 0 | 99 |
| architect | on-demand | 0 | 0 | 72 |
| scout | on-demand | — | — | — |

**Total pendiente: 204 tareas reales esperando.** Los agentes ahora pueden picar.

## Proyectos activos

| Proyecto | Repo | Fase | Agentes que lo tocan |
|---|---|---|---|
| panel-agentes | panel-agentes (https://github.com/CulitoDeVieja/panel-agentes) | Fase B ejecución | builder ✅ → auditor-ops pendiente |

## MODO MASTER activo

**Tarea madre:** panel-agentes (app Tauri Windows)

### Fase A — Planificación (en curso)

| Sub-tarea | Rol | Estado |
|---|---|---|
| architect-001, architect-002, architect-010 a 029 | architect | 21 pendientes (on-demand) |
| skills-curator-002 a 029 | skills-curator | 24 pendientes |
| builder-003 a 029 (planes) | builder | 21 completados ✅ |
| auditor-ops-003 a 029 | auditor-ops | 24 pendientes |
| orchestrator-002 (revisar + aprobar) | orchestrator | bloqueada por deps |

### Fase B — Ejecución (bloqueada hasta aprobar Fase A)

| Sub-tarea | Rol | Estado |
|---|---|---|
| builder-002 implementar panel-agentes | builder | ✅ completado — commit edf93b1 |
| auditor-ops-002 audit panel-agentes | auditor-ops | 🔒 bloqueada por builder-002 |

### Artefactos ya listos

- `PLAN-MASTER-panel-agentes.md` — plan maestro con stack, wireframes, tipos.
- `planificacion/panel-agentes/00-resumen.md` — aprobado.
- `planificacion/panel-agentes/mockup-visual.html` — 2 pantallas en HTML standalone.
- 4 manifests + 6 skills base (git-workflow, tareas-markdown, lazy-skill-loading, modo-master, anti-choques, skill-authoring).

### Próximo hito crítico

Destrabar skills-curator y auditor-ops para que procesen sus 48 tareas pendientes. Sin eso, Fase A no termina y Fase B sigue bloqueada.

## Decisiones pendientes del owner

(ninguna bloqueante)

## Reglas de escritura

- Solo el **orchestrator** edita `STATE.md`.
- Se actualiza cada vez que el orchestrator detecta cambio relevante en su loop.
- No es log histórico — es snapshot del presente. El log vive en `tareas/completado/`.
