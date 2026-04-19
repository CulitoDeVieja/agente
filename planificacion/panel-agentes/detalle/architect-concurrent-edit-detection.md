# Decisión: concurrent-edit-detection

**Decisión:** N/A v0.1 (read-only). En v0.2, si 2 agentes remotos editan simultáneo → git rebase resolverá; app muestra último pull.

## Alternativas
- Lock de archivos — sistema git no lo hace, confuso.
- Diff view en UI — v0.2.

## Justificación
Git ya maneja concurrency. UI refresh muestra último HEAD.
