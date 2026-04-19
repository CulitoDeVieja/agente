# react-day-picker v9 — date picker accesible

## TL;DR
- **react-day-picker v9** (ago 2024, última v9.14 feb 2026): date picker React unstyled-ish con CSS variables.
- Cambios clave: `date-fns` ahora dep (no peer), soporte UTC + Jalali/Buddhist/Hebrew calendars, ARIA WCAG 2.1.
- Breaking: `fromYear`/`toYear` → `startMonth`/`endMonth` con `Date` objects. Grid markup cambió → romper snapshot tests viejos.

## Detalles

### Install
```bash
bun add react-day-picker date-fns
# o shadcn (recomendado, incluye styling)
bunx shadcn@latest add calendar
```

### Uso básico (single date)
```tsx
import { DayPicker } from 'react-day-picker'
import 'react-day-picker/style.css'

<DayPicker mode="single" selected={date} onSelect={setDate} />
```

### Range
```tsx
<DayPicker
  mode="range"
  selected={range}
  onSelect={setRange}
  numberOfMonths={2}
/>
```

### Dropdown año/mes (nuevo v9)
```tsx
<DayPicker
  captionLayout="dropdown"
  startMonth={new Date(2020, 0)}
  endMonth={new Date(2030, 11)}
/>
```

### Features nuevas
- `hideWeekdayRow` / `hideNavigation`
- `aria-labelledby` para asociar con heading externo
- Calendarios no gregorianos (Jalali, Buddhist, Hebrew)
- CSS variables: `--rdp-accent-color`, `--rdp-day-height`, etc. — temas sin override deep

### Breaking v8 → v9
| v8 | v9 |
|---|---|
| `fromYear={2020}` | `startMonth={new Date(2020, 0)}` |
| `toYear={2030}` | `endMonth={new Date(2030, 11)}` |
| `modifiersClassNames` | igual pero diff de grid markup |
| CSS estaba en `react-day-picker/dist/style.css` | ahora `react-day-picker/style.css` |

### Integración shadcn
Issue conocido: shadcn `<Calendar>` rompió con v9 release (#4366), ya resuelto en versiones recientes. Re-generar con `bunx shadcn@latest add calendar` si levantás un proyecto en v9 nueva.

## Fuente
- https://daypicker.dev/upgrading
- https://daypicker.dev/changelog
- https://github.com/gpbl/react-day-picker/releases

## Aplicabilidad a panel-agentes
**Bajo, solo si agregamos filtros de fecha.** v0.1 no usa fechas en UI (las tabs son hoy, ayer, esta semana). Candidato para v0.2 si el usuario quiere:
- Filtrar tareas completadas por rango de fecha.
- Buscar logs entre fechas.
- Scheduling de agentes ("correr a las 09:00 cada día").

Si se agrega, usar shadcn `<Calendar>` (ya trae tema Tailwind). No instalar directo react-day-picker salvo caso custom. Mientras tanto, no agrega nada al bundle.
