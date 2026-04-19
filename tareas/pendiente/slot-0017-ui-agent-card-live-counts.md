# Slot 0017 · ui · agent-card-live-counts

**Fase:** Fase2-UI
**Rama de habilidad:** react
**Libre para tomar:** bot con skill React+Tailwind
**Asignado:** (pendiente)
**Rama git:** `slot-0017/agent-card-counts`

## Qué hacer

En `AgentCard.tsx`, mostrar 3 conteos en vivo: **pendientes / en curso / completadas** para el rol del card, leyendo de `useRepoData()`.

## Layout de la card

```
┌─────────────────────────────┐
│ #2 Builder        ●         │
│ VPS-2 · hace 6s             │
│                             │
│  12         1        87     │
│  pend      curso    comp    │
│                             │
│  ████████░░░░░ 86%          │
└─────────────────────────────┘
```

## Requisitos

- Componente usa hook existente `useRepoData()`.
- Filtrar tareas por `role === props.role`.
- Contar por `estado`.
- Progress bar = `comp / (pend + curso + comp)`.
- Números con color:
  - pend → `text-accent`
  - curso → `text-orange`
  - comp → `text-green`
- Last sync: tiempo relativo desde último refresh (usar date-fns `formatDistanceToNow`).
- Click en card → `onClick` prop para navegar a detalle.

## AC

- [ ] Card renderiza 3 números + barra de progreso.
- [ ] Click dispara `onClick(role)`.
- [ ] Test `AgentCard.test.tsx` con ≥4 casos (con datos, cero tareas, loading, error).
- [ ] Responsive: card no rompe layout en 300px ancho mínimo.

## Depende de:
- `slot-0003-feat-hook-use-repo-data.md` (completado/)

## Habilita
- slot-0018 (status dot color)
- slot-0019 (click to detail)
