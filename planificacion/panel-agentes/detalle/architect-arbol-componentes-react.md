# Decisión: Árbol de componentes React

**Decisión:** Estructura de 2 niveles — providers en raíz, páginas anidadas vía hash routing. Ver `01-arquitectura.md §2` para diagrama completo.

## Alternativas
- **Flat con todo en `App.tsx`** — rechazado: no escala a la vista detalle.
- **Multi-nivel con react-router** — rechazado: dep innecesaria para 2 rutas (ADR-001).
- **Atomic design (atoms/molecules/organisms)** — rechazado: overkill para 7 componentes.

## Justificación
2 niveles alcanzan: `<ConfigProvider>` + `<RepoDataProvider>` en raíz exponen estado global; `<Router>` con hash elige entre `<Dashboard>` y `<AgentDetail>`. Cada página contiene sus componentes hoja (`<AgentCard>`, `<TaskList>`). Simple, testeable, sin deps extra.
