# Slot 0020 · ui · agent-detail-view

**Fase:** Fase2-UI
**Rama de habilidad:** react
**Libre para tomar:** bot con skill React+Tailwind
**Asignado:** (pendiente)
**Rama git:** `slot-0020/agent-detail`

## Qué hacer

Vista detalle de un agente al clickear su card. 3 secciones: pendientes / en curso / últimas 10 completadas.

## Archivo a modificar

`panel-agentes/src/components/AgentDetail.tsx` (ya existe, expandir).

## Layout

```
┌──────────────────────────────────────────┐
│ ← Volver    #2 Builder      [Refresh ↻] │
│ VPS-2 · último signal hace 6s            │
├──────────────────────────────────────────┤
│ PENDIENTES (12)                          │
│   ○ slot-0016 ui-dashboard-grid [alta]  │
│   🔒 slot-0020 agent-detail [bloq deps] │
├──────────────────────────────────────────┤
│ EN CURSO (1)                             │
│   ● slot-0002 parse-task-markdown       │
├──────────────────────────────────────────┤
│ ÚLTIMAS 10 COMPLETADAS                   │
│   ✓ slot-0001 fetchTareasGitHub  6 min  │
└──────────────────────────────────────────┘
```

## Requisitos

- Prop `role: string` + `onBack: () => void`.
- Filtrar tareas via `useRepoData().byRole(role)`.
- 3 secciones con conteo en header.
- Tareas bloqueadas (`dependeDe.length > 0` con deps no completadas) → icon 🔒.
- Últimas 10 por `createdAt` descendente.
- Click en tarea → placeholder modal futuro.

## AC

- [ ] Componente muestra las 3 secciones con datos reales.
- [ ] Back button dispara `onBack()`.
- [ ] Tasks bloqueadas con icon distinto.
- [ ] Test `AgentDetail.test.tsx` con ≥4 casos (con datos, sin datos, con bloqueadas, con >20 pendientes scroll).
- [ ] Scroll interno si >20 tareas.

## Depende de:
- `slot-0003-feat-hook-use-repo-data.md`
- `slot-0016-ui-dashboard-4-cards-grid.md`

## Habilita
- Todos los slots de features que muestran detalle individual.
