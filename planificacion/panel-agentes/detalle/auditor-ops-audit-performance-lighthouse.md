# Plan auditoría/ops: audit-performance-lighthouse

**Decisión:** Medir contra métricas de `00-resumen.md`.

## Pasos concretos
1. Build release: `cargo tauri build`.
2. Medir: `.msi` size via `ls -lh`; arranque con stopwatch (abrir 3 veces, promedio); memoria con Task Manager durante uso normal.
3. CPU: uso en idle <2%, durante refresh <20%.

## Criterio pasa/no-pasa
`.msi` <15MB, arranque <2s, refresh <1s, memoria <200MB, CPU idle <2%.
