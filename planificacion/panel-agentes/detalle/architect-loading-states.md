# Decisión: Loading states

**Decisión:** Skeleton cards en primer fetch; spinner inline en botón ↻ durante refresh; sin loading global bloqueante salvo config inválida.

## Alternativas
- **Spinner global en cada fetch** — rechazado: tapa la data previa durante refresh.
- **Suspense + `React.lazy`** — rechazado v0.1: no hay code splitting útil con 7 componentes.
- **Progress bar superior (estilo YouTube)** — rechazado: UI más compleja sin ganancia para operación <1s.

## Justificación
Skeleton en cold-start evita flash de "0 tareas" en los contadores. Durante refresh mantener data vieja + spinner en el botón es mejor UX que vaciar la pantalla. Error → toast, no loading state.
