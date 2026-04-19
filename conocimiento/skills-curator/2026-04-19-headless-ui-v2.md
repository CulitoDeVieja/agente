# Headless UI v2 — React

## TL;DR
- v2 agrega anchor positioning built-in (Floating UI under the hood): Menu/Listbox/Combobox se posicionan solos.
- HTML form components nuevos: `Input`, `Select`, `Textarea`, `Label`, `Description`, `Fieldset`, `Legend` — manejan aria-*/id.
- Checkbox headless nuevo. Virtualización integrada con TanStack Virtual.

## Detalles

**Anchor positioning:**
```tsx
<Combobox anchor="bottom start">
  <ComboboxInput />
  <ComboboxOptions
    className="[--anchor-gap:4px] [--anchor-padding:8px]"
  >
    {...}
  </ComboboxOptions>
</Combobox>
```

Auto-ajusta si no cabe en viewport. CSS vars:
- `--anchor-gap` — distancia entre trigger y popover.
- `--anchor-offset` — offset manual.
- `--anchor-padding` — padding mínimo al viewport.

**Form components nuevos:**
```tsx
<Field>
  <Label>Name</Label>
  <Description>Your full name</Description>
  <Input />
</Field>
```

Auto-maneja `htmlFor`, `aria-describedby`, etc.

**Checkbox:**
```tsx
<Checkbox checked={enabled} onChange={setEnabled}>
  <svg>...</svg>  {/* render custom checkmark */}
</Checkbox>
```

**Virtualización Combobox:**
```tsx
<Combobox virtual={{ options: items }}>
  <ComboboxOptions>
    {({ option }) => <ComboboxOption value={option}>{option.name}</ComboboxOption>}
  </ComboboxOptions>
</Combobox>
```

Soporta 100k+ items sin problemas.

**Breaking:**
- `active` prop → reemplazado por `focus` prop.
- `@headlessui/tailwindcss` plugin deprecado → usar `data-*` attributes directo en Tailwind v3.2+.

## Fuente
- [Headless UI v2.0 for React - Tailwind blog](https://tailwindcss.com/blog/headless-ui-v2)
- [Release @headlessui/react v2.0.0](https://github.com/tailwindlabs/headlessui/releases/tag/@headlessui/react@v2.0.0)

## Aplicabilidad a panel-agentes
**No — preferir shadcn/ui.** Headless UI v2 es bueno pero overlap con shadcn/ui (que usa Radix).

Comparación rápida:
| | Headless UI v2 | shadcn/ui (Radix) |
|---|---|---|
| Primitives | ~10 | ~25 |
| Style model | own + Tailwind | Tailwind + CVA |
| Vendor lock | dep externa | copias código |
| Form components | sí (v2 nuevos) | dispersos |
| Virtualización | built-in | manual |

**Caso donde Headless UI ganaría:** si el proyecto fuera Tailwind Labs-first y no quisiéramos los ~25 components de Radix. Panel-agentes necesita más variedad (Dialog, Sheet, Table, DataTable, etc.) → Radix/shadcn son mejor fit.

**Nota útil:** la virtualización built-in en Combobox es impresionante. Si alguna vez necesitamos un search-combobox sobre 10k tareas históricas, Headless UI es opción concreta aunque el resto sea shadcn/ui.
