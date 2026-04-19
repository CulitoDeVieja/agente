# Decisión: sort-strategy

**Decisión:** Default: pendientes por prioridad (alta→baja), completadas por fecha descendente. Sin UI de sort v0.1.

## Alternativas
- User-configurable sort — innecesario inicialmente.
- Custom sort por agente — inconsistente.
- Sin sort (orden de archivo) — arbitrario.

## Justificación
Sort fijo en backend (comparador en Rust). Predecible.
