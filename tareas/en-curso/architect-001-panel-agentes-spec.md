# Tarea: Spec panel de control nativo (Windows)

**Rol:** architect
**Prioridad:** alta
**Creada:** 2026-04-19
**Proyecto:** panel-agentes (nuevo repo)

## Contexto
App nativa de escritorio (solo Windows, ejecutable `.exe`) que muestre progreso de cada agente y sus tareas.

**Plan master de referencia:** `/PLAN-MASTER-panel-agentes.md` en la raíz del repo. Tu spec debe ser coherente con él; podés profundizar pero no contradecir decisiones del plan.

## Stack definido por orchestrator
- **Tauri 2** (backend Rust + frontend web). Razón: exe <10MB, nativo, seguro, acceso directo a filesystem.
- **Frontend:** React + TypeScript + Vite.
- **Estilo:** Tailwind, paleta oscura minimal.
- **Build target:** Windows x64 (`.msi` o `.exe` portable).
- **No hay backend HTTP.** Lee directo del repo local clonado `C:/Users/Tony/AppData/Local/Temp/agente-repo/`.

## Qué debe mostrar
1. **Dashboard:** 4 cards (orchestrator / skills-curator / builder / auditor-ops), cada una con:
   - Última señal desde `STATE.md`
   - Cantidad de tareas en `pendiente/`, `en-curso/`, `completado/` (filtrado por rol)
2. **Vista por agente:** al clickear una card, lista de:
   - Tareas pendientes (título + prioridad)
   - Tarea en curso (con log parcial si hay)
   - Últimas 10 completadas (título + fecha de cierre extraída del log)
3. **Botón "refrescar"** → corre `git pull` en el repo local y actualiza la UI.
4. **(nice-to-have)** Botón "crear tarea" → formulario minimal que guarda archivo en `tareas/pendiente/`.

## Acceptance criteria
- [x] Spec escrita en `notas/architect/paso-001-panel-agentes-spec.md` (ubicada en repo `agente`, no en `panel-agentes` — ver log).
- [x] Incluye: estructura de carpetas, lista de componentes React, comandos Tauri, wireframe ASCII de las 2 vistas.
- [x] Plan de 3-5 pasos de implementación para Builder (5 pasos).

## Skills probables
- skills/spec-authoring-AC.md
- skills/tauri-app-architecture.md (a crear por skills-curator si no existe)

---

## Log del agente
- Spec escrita en `notas/architect/paso-001-panel-agentes-spec.md` (dentro de `agente`, no de `panel-agentes`).
  - Motivo: el repo `panel-agentes` fue creado público por error en esta sesión y quedó sin clonar por política. La estructura en `agente/notas/` ya existe (donde va ADR-001), así que la spec queda consolidada ahí hasta que el owner decida visibilidad del repo panel-agentes y builder clone para Paso 1.
- Detalle técnico completo en `planificacion/panel-agentes/01-arquitectura.md` (tarea architect-002). Este doc linkea y aporta el plan de 5 pasos para builder.
- Wireframes ASCII incluidos (Dashboard + Detalle).
- Pendiente de owner: visibilidad del repo `CulitoDeVieja/panel-agentes` (recomendación: privado).
