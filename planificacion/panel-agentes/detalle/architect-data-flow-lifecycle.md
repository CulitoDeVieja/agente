# Decisión: data-flow-lifecycle

**Decisión:** Unidireccional: refresh → git_pull → list_tasks+read_state paralelo → setState en RepoDataProvider → re-render hijos. Sin subscripciones reactivas.

## Alternativas
- Observables (RxJS) — overkill, 30KB extra.
- Event bus custom — reinventa context.
- Polling automático — revisado: solo on-demand via botón (v0.1 scope).

## Justificación
Refresh manual del user es el único trigger. Flujo lineal = fácil debug. Ver `01-arquitectura.md §7`.
