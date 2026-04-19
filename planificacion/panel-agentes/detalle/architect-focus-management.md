# Decisión: focus-management

**Decisión:** Focus trap en modal config. Focus visible via Tailwind `focus-visible:ring`. Al cerrar modal, focus vuelve al botón que lo abrió.

## Alternativas
- Sin focus management — keyboard users pierden contexto.
- Biblioteca (focus-trap-react) — 3KB, considerar si aparecen más modales.

## Justificación
Focus trap manual en 1 modal es 15 líneas. Migrar a lib si v0.2 agrega más.
