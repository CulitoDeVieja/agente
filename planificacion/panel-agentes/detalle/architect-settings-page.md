# Decisión: settings-page

**Decisión:** Modal en v0.1 (no página full). 3 campos: repoPath, refreshTimeoutMs, autoRefreshOnFocus. Botón "guardar" y "cancelar".

## Alternativas
- Settings page con tabs — innecesario para 3 opciones.
- Drawer lateral — ocupa permanente, poco uso.
- Config en file, editar manual — UX mala.

## Justificación
Modal con 3 campos cubre v0.1. Si v0.2 llega a 10+ opciones, migrar a page.
