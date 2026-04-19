# shadcn/ui + Tailwind v4 — 2026

## TL;DR
- shadcn/ui NO es library tradicional: copia/pega components a tu repo (radix-ui + tailwind debajo).
- Compat nativa con Tailwind v4 y React 19. Frameworks: Next.js, Vite, Laravel, React Router, Astro, TanStack Start.
- Cada component queda en `components/ui/` → podés modificarlo libremente. Sin vendor lock-in.

## Detalles

**Filosofía:**
- No es npm package con components exportados.
- CLI (`npx shadcn@latest add button`) copia el código del button a `components/ui/button.tsx`.
- Lo modificás como tuyo — es tu código, no una dependencia.

**Stack interno:**
- Radix UI para primitives accesibles (Dialog, Popover, Menu, etc.) — unstyled.
- Tailwind v4 para styling.
- CVA (class-variance-authority) para variants.
- `cn()` helper (clsx + tailwind-merge) en `lib/utils.ts`.

**Setup Vite + React + Tailwind v4:**
```bash
# 1. Instalar Tailwind v4
npm install tailwindcss @tailwindcss/vite

# 2. vite.config.ts
import tailwindcss from "@tailwindcss/vite";
export default { plugins: [react(), tailwindcss()] }

# 3. src/index.css
@import "tailwindcss";

# 4. Init shadcn
npx shadcn@latest init

# 5. Agregar components on-demand
npx shadcn@latest add button card dialog
```

**Estructura generada:**
```
src/
  components/ui/        ← components copiados (button, card, etc.)
  lib/utils.ts          ← cn() helper
```

**Componentes comunes para panel-agentes:**
- `button`, `card`, `badge`, `tabs` — base.
- `dialog`, `sheet` — modales/sidebars.
- `table`, `data-table` — listas de tareas.
- `skeleton` — loading states.
- `toast` (via Sonner) — notificaciones.

## Fuente
- [shadcn/ui Vite setup](https://ui.shadcn.com/docs/installation/vite)
- [shadcn/ui Tailwind v4 compatibility](https://ui.shadcn.com/docs/tailwind-v4)
- [Setting up React 19 + Tailwind v4 + shadcn/ui - Medium](https://medium.com/@sumitnce1/setting-up-react-19-with-tailwind-css-v4-and-shadcn-ui-without-typescript-b47136d335da)

## Aplicabilidad a panel-agentes
**Sí — alta prioridad para v0.2.** Panel-agentes actualmente no tiene design system. Beneficios:
1. **Accesibilidad gratis** (Radix primitives ya cumplen WCAG).
2. **Consistencia** — theme tokens centralizados.
3. **Velocidad** — no reinventar Button, Card, Dialog.
4. **Sin lock-in** — código propio, no dep externa.

**Plan para v0.2:**
- Instalar shadcn/ui.
- Reemplazar AgentCard custom por `<Card>` + `<Badge>` + `<Button>`.
- Dialog de detalle de tarea con `<Dialog>`.
- Lista de tareas con `<DataTable>` (integra con `@tanstack/table`).

**Alternativas descartadas:**
- HeadlessUI (Tailwind Labs) → menos components.
- Radix UI standalone → más trabajo manual de estilos.
- Chakra/MUI → opinionated theme, reñido con Tailwind.

**Notar para builder:** la skill `react-tailwind-setup.md` ya usa Tailwind v4 — compatible con shadcn/ui out of the box.
