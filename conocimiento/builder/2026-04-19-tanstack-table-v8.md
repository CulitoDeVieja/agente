# TanStack Table v8 — headless data grid

## TL;DR
- **TanStack Table v8** (ex React Table) = motor headless: hooks que hacen el estado (sort/filter/pagination/selection) y vos controlás 100% el markup.
- Framework-agnostic (React/Vue/Solid/Svelte). TypeScript-first, `ColumnDef` fuertemente tipada.
- Escala a 100k+ filas client-side para la mayoría de ops comunes.

## Detalles

### Install
```bash
bun add @tanstack/react-table
```

### Setup mínimo
```tsx
import {
  useReactTable, getCoreRowModel, getSortedRowModel, flexRender,
  type ColumnDef,
} from '@tanstack/react-table'

type Agent = { id: number; role: string; status: string; lastSignal: string }

const columns: ColumnDef<Agent>[] = [
  { accessorKey: 'id', header: '#' },
  { accessorKey: 'role', header: 'Rol' },
  {
    accessorKey: 'status',
    header: 'Estado',
    cell: ({ getValue }) => <StatusBadge value={getValue<string>()} />
  },
  { accessorKey: 'lastSignal', header: 'Última señal' },
]

const table = useReactTable({
  data,
  columns,
  getCoreRowModel: getCoreRowModel(),
  getSortedRowModel: getSortedRowModel(),
})
```

### Features opt-in (tree-shakeable)
```ts
getFilteredRowModel      // column filters
getGroupedRowModel       // agrupación
getPaginationRowModel    // paginación client
getExpandedRowModel      // rows expandibles
getFacetedRowModel       // facets para filters
```
Solo importás los que usás → bundle lean.

### Row selection
```ts
const table = useReactTable({
  ...,
  enableRowSelection: true,
  state: { rowSelection },
  onRowSelectionChange: setRowSelection,
})

table.getSelectedRowModel().rows   // para bulk actions
```

### Server-side mode
Si los datos son muchos, pasás `manualSorting: true` + `manualFiltering: true` + `pageCount` y hacés vos la query. La librería solo maneja UI state.

### Virtualización
No built-in. Combinar con `@tanstack/react-virtual` para tablas de >10k rows visibles.

### shadcn/ui data-table
```bash
bunx shadcn@latest add data-table
```
Genera componente con TanStack Table + styling tailwind pre-armado.

## Fuente
- https://tanstack.com/table/v8
- https://github.com/TanStack/table
- https://tanstack.com/table/v8/docs/guide/sorting

## Aplicabilidad a panel-agentes
**Sí, para la tabla principal de agentes y lista de tareas.**
- Tabla agentes (pocas filas, ~10 max) → core + sort suficiente.
- Tabla tareas (puede tener cientos en pendiente/completado) → + filter + pagination.
- Row selection para bulk ops futuras (restart múltiple, kill).
- Usar `bunx shadcn add data-table` para arrancar con tailwind integrado.
- No necesitamos virtualización en v0.1 — agregar solo si logs viewer pasa los 1k.
