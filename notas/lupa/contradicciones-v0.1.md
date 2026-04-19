# Lupa · Contradicciones detectadas v0.1

**Inspector:** Lupa (dual orchestrator)
**Fecha:** 2026-04-19
**Alcance:** Fase A + Fase B panel-agentes.

---

## 1. Mockups — ¿cuál es el canónico?

**Problema:** hay 4 mockups (`v1`, `v2`, `v3-final`, `v4`). `v3-final` dice "FINAL" pero después hicimos `v4` con agentes numerados. Ambiguo para builder.

**Resolución Lupa:**
- **v4 = canónico** (más reciente + respeta la directiva "nombrar con número + función").
- v1, v2, v3-final → **archivar** en `planificacion/panel-agentes/archivo/mockups-previos/`.
- Agregar nota en `v4.html` diciendo "este es el canónico, referencia builder".

## 2. Divergencias código (auditor-ops-002 audit)

**Detectadas:**
- `Command::new("git")` vs `git2` crate → decisión de builder, no es bug.
- Parser markdown manual vs `pulldown-cmark` → decisión de builder.
- `list_tasks(rol: String)` con `"all"` mágico vs `Option<String>`.

**Resolución Lupa:**
- 1 y 2 → aceptar como decisiones de diseño (menos deps, más control). **Documentar en ADR**.
- 3 → **abrir tarea builder-fix** para cambiar `rol: String` a `rol: Option<String>` (idiomatic Rust).

## 3. Doc `03-implementacion.md` vs realidad

**Problema:** `03-implementacion.md` lista 12 pasos planeados. El builder ejecutó en 1 commit gigante (27 archivos). No está mal, pero rompe la trazabilidad paso-a-paso.

**Resolución:** post-hoc — aceptar. El builder demostró velocidad. Para próximos features, pedir commits por componente (regla ya existe en `anti-choques.md`).

## 4. Skills-curator edita INDEX con commits gigantes

**Problema:** violación de regla `anti-choques.md` R5 "un commit por tarea". Skills-curator hace 50 cambios en 1 commit.

**Resolución:** ya hay tarea `skills-curator-106-push-por-tarea.md` creada. Seguir.

## 5. Stack decisions — contradicciones entre planificaciones

Revisé `PLAN-MASTER-panel-agentes.md` + `01-arquitectura.md` + decisiones 010-079 architect + evoluciones.

**No hay contradicciones stack.** Tauri 2 + React 18 + TS + Tailwind v4 confirmado en todos.

**Potencial ambigüedad:** `zustand-vs-context-decision` (skill 021) + ADR-001 "sin state manager". ADR-001 gana. La skill es referencia.

## 6. Tareas bloqueadas sin sentido

- `builder-002-implementar-panel-agentes` → ya **completada** (está en `completado/`). No es contradicción, chequeé.
- `auditor-ops-002-audit-panel-agentes` → completada también. OK.

## 7. Curaduría Core/Backlog en conocimiento/

**Revisé INDICE.md:** la clasificación orchestrator quedó razonable. Sin contradicciones con evoluciones realizadas.

---

## Tareas de fix generadas

| ID | Severidad | Descripción |
|----|-----------|-------------|
| builder-fix-001 | media | `list_tasks` cambiar `rol: String` → `rol: Option<String>` |
| orchestrator-012 | baja | Archivar mockups v1/v2/v3-final en `archivo/`, marcar v4 canónico |
| builder-adr-001 | baja | Escribir ADR justificando Command::git y parser markdown manual |

Ninguna crítica. Código listo para gate de release si se cierran estas 3.
