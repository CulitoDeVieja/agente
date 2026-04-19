# PLAN FINAL v0.2 — Panel Agentes (ampliación)

**Fecha:** 2026-04-19
**Orchestrator:** Zeus
**Origen:** v0.1 funcional + bugs cerrados. Ahora crecemos.

---

## 1. Roster de agentes

| # | Rol | Host | Modo |
|---|---|---|---|
| 0 | Orchestrator / Zeus | local | loop 1min |
| 1 | Skills-Curator | VPS-1 | loop 1min |
| 2 | Builder | VPS-2 | loop 1min |
| 3 | Auditor-Ops | VPS-3 | loop 1min |
| 4 | **Lupa** | Claude Code session | on-demand |
| 5 | **Estado** | Claude Code session | on-demand |
| 6 | **Tester** | Claude Code session | on-demand |

**#4/5/6 ejecutan TODAS sus tareas en una sola sesión** (no hay loop). El owner abre sesión, pega prompt, corren las 100 de un tirón.

## 2. Estrategia git — ramas anti-choque

Cada agente trabaja en su branch propia:

```
main                        ← default, protegida
├── agent-1/skills          (skills-curator)
├── agent-2/builder         (builder)
├── agent-3/auditor-ops     (auditor-ops)
├── agent-4/lupa            (lupa)
├── agent-5/estado          (estado)
└── agent-6/tester          (tester)
```

**Flujo:**
1. Agente corre ciclo → lee `tareas/pendiente/<agent-X>/*.md` asignadas a su branch.
2. Trabaja en worktree de su branch.
3. Push a su branch → abre PR a `main`.
4. **Orchestrator revisa el PR** → merge o pide re-edit.

Nunca dos agentes tocan el mismo archivo en el mismo ciclo. El PR resuelve conflictos mínimos.

## 3. Qué construimos en v0.2

Ampliar panel-agentes v0.1. Features:

- **UX:** cmd palette (⌘K), filter chips, toast notifications, settings modal, help overlay, dark/light toggle persistente.
- **Agente #4/5/6 nuevas vistas:** lupa-report view, estado-dashboard, tester-runs.
- **Bugs live:** vista que lea `notas/lupa/bugs.json` y renderice el estado.
- **Notificaciones críticas:** sistema tray icon + notif nativa Windows cuando un agente reporta error.
- **Release pipeline:** CI/CD para `.msi` auto en tag.
- **Tests E2E** con Playwright.
- **PWA/web version** (fase exploratoria).
- **Doc completa:** guía instalación, FAQ, troubleshooting, API reference.

## 4. Distribución 100 tareas × agente (total 600)

| Agente | Foco | Ejemplos |
|---|---|---|
| #1 Skills-Curator | Skills para v0.2 features | shadcn/ui install, sonner toast setup, cmdk, zustand patterns, lucide icons, radix primitives |
| #2 Builder | Implementación React+Rust | componentes UI, hooks, comandos Rust, parsers, services |
| #3 Auditor-Ops | QA + deploy pipeline | CI GH Actions, code signing, release automático, monitoring, docs |
| #4 Lupa | Audit dinámico + reports | revisar cada PR, medir métricas, detectar regresiones, mantener bugs.json |
| #5 Estado | Dashboard STATE.md + live | consolidar STATE, generar snapshots JSON, refrescar progreso, visualizaciones |
| #6 Tester | Suites completas | unit adicional, integration, E2E Playwright, smoke tests Windows, manual scripts |

Cada agente recibe su lote de 100 en `tareas/pendiente/agent-N/` con prefijo de branch.

## 5. Gate de arranque (antes de codear)

Ya cumplidos:

- [x] Tests vitest: **8/8 verdes**.
- [x] npm audit: **0 vulnerabilidades**.
- [x] Default branch: `main`.
- [x] `src-tauri/capabilities/` presente.
- [x] Icono placeholder.
- [x] CI workflow en `.github/workflows/ci.yml`.
- [x] ADR-001 documentado.
- [x] ALL PASS activo en los 4 hosts.
- [x] **bugs.json: 14 registrados, 14 cerrados. 0 abiertos.**

**Green light.** Arrancamos.

## 6. Flujo de review del orchestrator

Para cada PR de agente:

1. `git fetch origin agent-N/nombre`
2. Leer diff + tests.
3. 3 opciones:
   - **✅ Merge directo** → `git merge --no-ff` + push main.
   - **🔁 Pedir re-edit** → comentario en PR + crear tarea de fix para ese agente.
   - **📝 Tocar yo** → cherry-pick + ajustar + merge.

El orchestrator tiene **criterio propio**. No pregunta al owner salvo blast radius alto (cambio de stack, nueva dep pesada, decisión estratégica).

## 7. Comunicación Owner ↔ Zeus

- Owner ve progreso en:
  - `STATE.md` (snapshot)
  - `notas/lupa/bugs.json` (bugs live)
  - `planilla-tareas.html` (UI navegable)
  - PRs abiertos en GitHub (https://github.com/CulitoDeVieja/panel-agentes/pulls)
- Owner interrumpe solo para directivas estratégicas.
- Zeus reporta al owner solo:
  - Hitos grandes (PR mergeado, release v0.2).
  - Bugs críticos que aparecen.
  - Cuando cola queda vacía.

## 8. Release v0.2 criterio

- Todos los PR de los 6 agentes mergeados a main.
- CI verde en Actions.
- Tests: unit 100% + E2E smoke OK.
- Lupa firma `GATE-PASS-v0.2.md`.
- Tester corre `test-plan.md` actualizado.
- Auditor-ops genera `.msi` firmado + sube release GitHub.

## 9. Cronograma estimado (a ritmo de los agentes actuales)

- Asignación tareas: **hoy** (siguiente ciclo orchestrator).
- Ejecución #1/#2/#3 (loop): **7-14 días**.
- Ejecución #4/#5/#6 (on-demand, un tirón): **3-5 sesiones de 20 min c/u**.
- Review + merges: **paralelo durante ejecución**.
- Release v0.2: **2-3 semanas**.

---

## Arrancamos

Este plan es final salvo que el owner diga lo contrario en 1 ciclo. Si no hay objeción:
1. Orchestrator genera 600 tareas `tareas/pendiente/agent-N/...` en próxima ronda.
2. Agentes tomarán en sus ciclos.
3. Primer PR esperado en 24-48hs.
