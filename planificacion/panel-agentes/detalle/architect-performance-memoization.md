# Decisión: Performance y memoization

**Decisión:** Memoizar solo donde el profiler lo pida. Split de contexts (Config vs RepoData) como única optimización preventiva. `React.memo` en `<AgentCard>` porque se renderizan 4.

## Alternativas
- **Memoizar todo con `React.memo` + `useMemo`** — rechazado: código más noisy, bug surface mayor, ganancia imperceptible a esta escala (4 cards, <50 tareas).
- **Virtualizar `<TaskList>` con react-window** — rechazado v0.1: <10 items por lista; agregar si v0.2 muestra 100+ completadas.
- **Web workers para parser** — rechazado: parser Rust ya es nativo.

## Justificación
Regla: medir primero. Dataset de 4 agentes × <50 tareas cada uno no justifica overhead de memoization. Split de context sí porque Config cambia raramente (una vez a la vida) vs data (cada ↻).
