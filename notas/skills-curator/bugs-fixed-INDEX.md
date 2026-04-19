# Skills-Curator → Zeus · Bug audit INDEX.md

**De:** Skills-Curator (#1, VPS-1)
**Para:** Zeus (orchestrator)
**Fecha:** 2026-04-19
**Origen:** orden del owner "busca bugs y resolvelos. coordina con zeus".

---

## Bugs detectados y resueltos (INDEX.md)

Audit automático entre `ls agentes/skills/` y `grep` de `INDEX.md`:

### Missing (1 bug, severidad baja)
- `test-running.md` — referenciada en INDEX (Auditor+Ops), nunca existió en disco (sin commits).
  - **Fix:** reemplazada por dos skills existentes que cubren el rol:
    - `test-rust-cargo.md` (cargo test).
    - `react-testing-vitest.md` (bun/npm test con vitest).

### Orphans (4 bugs, severidad baja — skills existían sin entrada en INDEX)
- `all-pass.md` → agregada a "Skills base".
- `evolucion.md` → agregada a "Skills base".
- `fase-b-ejecucion.md` → agregada a "Skills base".
- `permisos-preaprobados.md` → agregada a "Skills base".

**Nota:** `nodejs-express.md` aparece solo como ejemplo en la sección "Convención de nombres" — NO es bug.

---

## Resumen

| Bug ID | Severidad | Estado |
|---|---|---|
| SC-INDEX-001 | baja | cerrado (commit `711ef3d`) |
| SC-INDEX-002 | baja | cerrado (commit `711ef3d`) |
| SC-INDEX-003 | baja | cerrado (commit `711ef3d`) |
| SC-INDEX-004 | baja | cerrado (commit `711ef3d`) |
| SC-INDEX-005 | baja | cerrado (commit `711ef3d`) |

**5 bugs cerrados en 1 commit** (todos en el mismo archivo INDEX.md, fix atómico aceptable por R5 anti-choques — ajustar múltiples líneas del mismo archivo no viola la regla).

---

## Siguiente paso sugerido a Zeus

Si querés que agregue esto a `notas/lupa/bugs.json` o generes un `bugs.json` propio de skills-curator, dejá tarea `skills-curator-*` en pendiente y lo tomo.

Otras áreas donde podría auditar:
- Skills duplicadas (mismo tema, archivos distintos).
- Skills con tech obsoleta (Tailwind v3, React 17).
- Skills con enlaces muertos.

Esperando orden.
