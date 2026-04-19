# Slot 0016 · ui · dashboard-4-cards-grid

**Fase:** Fase2-UI
**Rama de habilidad:** react
**Libre para tomar:** bot con skill React+Tailwind
**Asignado:** (pendiente)
**Rama git:** `slot-0016/dashboard-grid`

## Qué hacer

Construir el Dashboard principal del panel con **grid de 4 cards** (orchestrator, skills-curator, builder, auditor-ops) + sidebar + header. Referencia visual: `planificacion/panel-agentes/mockup-visual-v4.html`.

## Archivo a modificar

`panel-agentes/src/components/Dashboard.tsx` (ya existe, expandir).

## Layout esperado

```
┌──────────────────────────────────────────┐
│ Header · Panel Agentes · [↻] [⌘K] [live] │
├──────┬───────────────────────────────────┤
│ Side │  ┌────┐ ┌────┐ ┌────┐ ┌────┐     │
│ bar  │  │ #0 │ │ #1 │ │ #2 │ │ #3 │     │
│ 9    │  └────┘ └────┘ └────┘ └────┘     │
│ items│                                   │
├──────┴───────────────────────────────────┤
│ Status bar: master @ commit · last sync  │
└──────────────────────────────────────────┘
```

## Requisitos

- Grid `grid-cols-4 gap-4` con responsive fallback a `grid-cols-2` en <900px.
- Cada card usa `<AgentCard role={...} />` (ya existe).
- Integrar con `useRepoData()` para obtener conteos reales.
- Loading state → 4 `<LoadingSkeletonCard />`.
- Empty state → si cero tareas → "Sin datos. Click ↻".
- Error boundary envolviendo todo el Dashboard.

## AC

- [ ] Dashboard renderiza 4 cards con datos reales de `useRepoData`.
- [ ] Loading skeletons durante fetch inicial.
- [ ] Error boundary captura errores de render.
- [ ] Test `Dashboard.test.tsx` con ≥3 casos (loading, empty, con datos).
- [ ] Screenshot dark mode antes/después.

## Depende de:
- `slot-0003-feat-hook-use-repo-data.md` (completado/)

## Habilita
- slot-0017 (AgentCard live counts)
- slot-0020 (Agent detail view)
