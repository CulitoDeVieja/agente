# Plan auditoría/ops: smoke-test-refresca

**Decisión:** Checklist manual post-build. VM Windows limpia.

## Pasos concretos
1. Instalar MSI en VM Windows.
2. Abrir app: debe mostrar Dashboard.
3. Ejercitar: click cada card, back, refresh, cerrar y reabrir.
4. Verificar: estado post-reabrir consistente con repo actual.

## Criterio pasa/no-pasa
Cero crashes. UI responde en <300ms a cada acción. Al reabrir, carga correctamente.
