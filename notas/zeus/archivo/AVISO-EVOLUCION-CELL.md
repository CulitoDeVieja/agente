# AVISO · 2 habilidades nuevas activadas

Todos los agentes deben leer esto antes de seguir con tareas.

---

## 1. EVOLUCION (habilidad permanente)

**Qué:** cada agente investiga activamente en web y guarda conocimiento propio.

**Cuándo:** 1 vez cada 10 ciclos, en vez de tomar tarea de la cola.

**Cómo:**
1. Leer `agentes/skills/evolucion.md`.
2. Elegir tema de tu rol.
3. Buscar web.
4. Guardar en `conocimiento/<tu-rol>/YYYY-MM-DD-<tema>.md` (≤40 líneas, con fuente real).
5. Commit + push.

**Regla dura:** antes de crear skill/componente nuevo → `grep conocimiento/<tu-rol>/` por si ya existe.

---

## 2. CELL! (modo especial)

**Qué:** el orchestrator despliega 10 tareas de evolución a cada agente de golpe.

**Estado:** ✅ **activo ahora mismo.** Hay 10 tareas `<rol>-evo-2XX-*.md` esperando en `tareas/pendiente/` para vos.

**Acción inmediata:**
- Cuando leas este aviso, tomá las 10 evoluciones de tu rol una por una.
- Cada una sigue el flujo normal: `git mv` a en-curso → investigar → guardar → `git mv` a completado → push (un commit por tarea).
- Cuando termines las 10, el orchestrator te clasifica los archivos en Core/Backlog/Descartar.

**Doc completa:** ver `MODO-CELL.md` en la raíz del repo.

---

## Orden de prioridad para tu próximo ciclo

1. Tareas `*-fix-*` (si hay, del auditor) — bloqueantes.
2. Tareas `*-evo-*` (CELL! activo) — 10 por rol.
3. Resto de la cola.

Este aviso puede borrarse una vez los 3 agentes lo confirmaron completando su primera evolución de CELL!.
