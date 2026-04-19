# Slot 0086 · feat · filter-chips-tareas

**Fase:** Fase3-Features
**Rama de habilidad:** react
**Libre para tomar:** bot con skill React
**Asignado:** (pendiente)
**Rama git:** `slot-0086/filter-chips`

## Qué hacer

Componente `<FilterChips />` que muestra chips togglables (Todas · Pendientes · En curso · Completadas · Bloqueadas) y filtra la lista de tareas mostrada.

## Layout

```
[ Todas · 120 ]  [ Pendientes · 87 ]  [ En curso · 3 ]  [ Completadas · 28 ]  [ Bloqueadas · 2 ]
    ●                                         (activa)
```

## API

```ts
type FilterKey = "all" | "pend" | "curso" | "done" | "blocked";

type FilterChipsProps = {
  active: FilterKey;
  counts: Record<FilterKey, number>;
  onChange: (key: FilterKey) => void;
};
```

## Requisitos

- Chips pill-shaped con Tailwind.
- Active chip → border-accent + bg-accent/15.
- Count como sup text.
- Click cambia `active` via `onChange`.
- Keyboard: arrows left/right navegan, Enter activa.
- Responsive: si overflow → scroll horizontal con fade-edge.

## AC

- [ ] Renderiza 5 chips.
- [ ] Click dispara `onChange`.
- [ ] Counts actualizan según datos.
- [ ] Arrow nav keyboard.
- [ ] Test `FilterChips.test.tsx` con ≥4 casos.

## Depende de: (ninguna)

## Habilita
- slot-0087 (combined logic) — aplicar filtros en `TaskList`.
- slot-0088 (save preset)
