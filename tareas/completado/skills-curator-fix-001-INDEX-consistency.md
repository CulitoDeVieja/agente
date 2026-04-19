# Tarea: Fix INDEX consistency (orphans + missing)

**Rol:** skills-curator
**Prioridad:** media
**Creada:** 2026-04-19
**Origen:** orden del owner ("busca bugs y resolvelos")

## Qué hacer
Arreglar 5 bugs detectados en `agentes/skills/INDEX.md`:

### Missing (listada en INDEX, no en disco)
- `test-running.md` — nunca existió. Reemplazar referencia por `test-rust-cargo.md` + `react-testing-vitest.md` (ya documentan el caso) o remover línea.

### Orphans (en disco, no en INDEX)
- `all-pass.md` — base skill (bypass permissions). Agregar a "Skills base".
- `evolucion.md` — skill permanente. Agregar a "Skills base".
- `fase-b-ejecucion.md` — skill operativa. Agregar a "Skills base" o sección propia.
- `permisos-preaprobados.md` — skill auxiliar. Agregar a "Skills base" o referenciar desde all-pass.

## AC
- [ ] INDEX.md sin referencias muertas.
- [ ] Todas las skills en disco listadas en INDEX.
- [ ] Nota a Zeus en `notas/skills-curator/bugs-fixed-INDEX.md`.

## Depende de: (ninguna)
