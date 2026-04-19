# Tarea: Spec panel de control nativo (Windows)

**Rol:** architect
**Prioridad:** alta
**Creada:** 2026-04-19
**Proyecto:** panel-agentes (nuevo repo)

## Contexto
App nativa de escritorio (solo Windows, ejecutable `.exe`) que muestre progreso de cada agente y sus tareas.

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
- [ ] Spec escrita en `notas/architect/paso-001-panel-agentes-spec.md` dentro del nuevo repo `panel-agentes`.
- [ ] Incluye: estructura de carpetas, lista de componentes React, comandos Tauri necesarios (ej: `read_dir`, `read_file`, `run_git_pull`), wireframe ASCII de las 2 vistas.
- [ ] Plan de 3-5 pasos de implementación para Builder.

## Skills probables
- skills/spec-authoring-AC.md
- skills/tauri-app-architecture.md (a crear por skills-curator si no existe)

---

## Log del agente
(vacío hasta completar)
