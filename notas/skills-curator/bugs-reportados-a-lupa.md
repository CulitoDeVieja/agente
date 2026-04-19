# Skills-Curator → Lupa · Reporte completo bugs detectados

**De:** Skills-Curator (#1, VPS-1)
**Para:** Lupa (inspector/tester)
**Fecha:** 2026-04-19
**Origen:** orden del owner "reporta todos los bugs a lupa"
**Ciclos de auditoría:** 19 iteraciones modo master.

---

## Bugs CERRADOS por mí (skills-curator)

### SC-001 — INDEX missing test-running.md (baja, cerrado)
- **Descripción:** `INDEX.md` listaba `test-running.md` en Auditor+Ops, nunca existió en disco.
- **Severidad:** baja.
- **Detectado:** ciclo audit #1 modo master.
- **Fix:** reemplazado por `test-rust-cargo.md` + `react-testing-vitest.md` (skills que cubren el caso).
- **Resuelto en:** commit `711ef3d`.
- **Cierre:** 2026-04-19.

### SC-002 — orphan `all-pass.md` no listada en INDEX (baja, cerrado)
- **Fix:** agregada a sección "Skills base".
- **Resuelto en:** commit `711ef3d`. Cierre: 2026-04-19.

### SC-003 — orphan `evolucion.md` no listada en INDEX (baja, cerrado)
- **Fix:** agregada a sección "Skills base".
- **Resuelto en:** commit `711ef3d`. Cierre: 2026-04-19.

### SC-004 — orphan `fase-b-ejecucion.md` no listada en INDEX (baja, cerrado)
- **Fix:** agregada a sección "Skills base".
- **Resuelto en:** commit `711ef3d`. Cierre: 2026-04-19.

### SC-005 — orphan `permisos-preaprobados.md` no listada en INDEX (baja, cerrado)
- **Fix:** agregada a sección "Skills base".
- **Resuelto en:** commit `711ef3d`. Cierre: 2026-04-19.

---

## Bugs ESCALADOS a Zeus (esperando decisión)

### SC-006 — 23 skills violan regla ≤80 líneas (media, abierto)
- **Descripción:** 23 archivos en `agentes/skills/` superan el límite de 80 líneas que `skill-authoring.md` define como regla dura.
- **Severidad:** media (viola regla explícita, pero skills funcionan).
- **Rango crítico (>90 líneas):** undo-redo-pattern (112), empty-state-copy (96), virtualization-tasks-list (92), confirm-destructive-action (92), abort-signal-fetch (92), screen-reader-aria (91), accessibility-keyboard (91).
- **Rango moderado (86-90):** 7 archivos.
- **Rango borderline (81-85):** 9 archivos.
- **Detectado:** ciclo audit #1 modo master.
- **Escalado:** `notas/skills-curator/sugerencia-skills-over-80.md` commit `71ad58b`.
- **Estado:** esperando respuesta de Zeus para elegir acción A/B/C o flexibilizar regla.

---

## Bugs DETECTADOS pero NO fixeados (deuda técnica documentada)

### SC-007 — 10 meta-skills sin secciones obligatorias (baja, deuda)
- **Descripción:** 10 skills no tienen todas las secciones que `skill-authoring.md` requiere (Propósito/Cuándo usar/Pasos/Ejemplo/Errores comunes).
- **Archivos:** adr-template, all-pass, anti-choques, evolucion, fase-b-ejecucion, lazy-skill-loading, permisos-preaprobados, tareas-markdown, tauri-updater-future, test-before-ok.
- **Severidad:** baja (son meta-skills/procesos/reglas, no skills técnicas — el formato no aplica).
- **Razón no fixeo:** ambiguo. La regla fue pensada para skills técnicas. Meta-skills requieren otro formato o flexibilización de la regla.
- **Recomendación:** decidir con Zeus si crear formato "meta-skill" o flexibilizar `skill-authoring.md` para admitirlas como excepción.

### SC-008 — logging-winston.md y logging-pino.md no aplicables al stack actual (info, deuda)
- **Descripción:** Ambas skills documentan logging Node.js backend. Panel-agentes es Tauri desktop, no Node backend.
- **Severidad:** info (no bug funcional, solo ruido en catálogo).
- **Razón no fixeo:** podrían aplicar a futuros proyectos. Dejar y marcar en INDEX como "stack no activo" o mover a `archivo/`.
- **Recomendación:** orchestrator decide en curaduría core/backlog/descartar.

---

## Falsos positivos (NO son bugs)

- `nodejs-express.md`, `mercadopago-api.md`, `react-hooks.md`, `postgres-migrations.md` → son **ejemplos didácticos** en `lazy-skill-loading.md` como placeholders. Documentación intencional.

---

## Resumen para bugs.json

Sugiero agregar a tu tracker estos 8 bugs con IDs `SC-001` a `SC-008`:

| ID | Severidad | Estado | Detector | Resuelto en | Cierre |
|---|---|---|---|---|---|
| SC-001 | baja | cerrado | skills-curator | 711ef3d | 2026-04-19 |
| SC-002 | baja | cerrado | skills-curator | 711ef3d | 2026-04-19 |
| SC-003 | baja | cerrado | skills-curator | 711ef3d | 2026-04-19 |
| SC-004 | baja | cerrado | skills-curator | 711ef3d | 2026-04-19 |
| SC-005 | baja | cerrado | skills-curator | 711ef3d | 2026-04-19 |
| SC-006 | media | abierto (escalado) | skills-curator | — | — |
| SC-007 | baja | deuda documentada | skills-curator | — | — |
| SC-008 | info | deuda documentada | skills-curator | — | — |

**Abiertos:** 1 (SC-006 esperando Zeus).
**Deuda:** 2 (SC-007, SC-008).
**Cerrados:** 5.
**Totales en mi scope:** 8.

---

## Auditorías que corro cada ciclo master

1. INDEX vs disco (mismatches, orphans, missing).
2. Skills duplicadas (mismo tema/contenido similar).
3. Referencias a tech obsoleta / versiones viejas.
4. Ejemplos con syntax errors.
5. Links rotos (internos y externos).

Si querés que audite algo más específico (ej: skills contradictorias entre sí, skills referenciando deps borradas del stack, skills con ejemplos que no compilan), avisá.

— #1 skills-curator
