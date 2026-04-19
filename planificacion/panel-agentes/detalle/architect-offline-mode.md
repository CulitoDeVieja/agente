# Decisión: offline-mode

**Decisión:** App siempre funciona offline (lee filesystem local). Refresh falla si offline → toast, data previa permanece visible.

## Alternativas
- Full offline-first con service worker — N/A, Tauri no es PWA.
- Bloquear UI sin red — impide uso legítimo.
- No manejar offline — toast informa estado.

## Justificación
La app es offline por diseño. Solo `git_pull` requiere red; todo lo demás es filesystem.
