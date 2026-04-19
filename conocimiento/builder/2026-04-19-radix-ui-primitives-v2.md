# Radix UI Primitives — estado "v2" (abril 2026)

## TL;DR
- **No hay v2 mayor** de Radix Primitives: todo sigue en línea 1.x (algunos utilitarios como `Label` en 2.x).
- Soporte completo **React 19** confirmado (peer: `^16.8 || ^17 || ^18 || ^19`).
- Novedades 2025: primitives unstable `PasswordToggleField` y `OneTimePasswordField`.

## Detalles

### Instalación modular
```bash
bun add @radix-ui/react-dialog @radix-ui/react-dropdown-menu @radix-ui/react-tooltip
```
Cada primitive es paquete independiente — no instalar el meta-paquete, tree-shaking pobre.

### Shadcn/ui como wrapper preferido
En 2026 el patrón estándar es **shadcn/ui** (copiar+editar componentes que usan Radix debajo):
```bash
bunx shadcn@latest add dialog dropdown-menu tooltip
```
- Obtenés el componente Tailwind-styled en tu repo.
- Ownership completo del código (no es dep).
- Radix se sigue actualizando bajo el capó.

### Primitives útiles para panel-agentes
| Primitive | Uso en panel |
|---|---|
| `Dialog` | modal "detalles de agente" |
| `DropdownMenu` | acciones por agente (restart, kill, logs) |
| `Tooltip` | hover info en íconos de estado |
| `Tabs` | pestañas entre vistas (agentes / tareas / logs) |
| `ScrollArea` | logs viewer con scroll custom |
| `Toast` | notificaciones (fallback a `sonner` si preferís) |
| `ContextMenu` | click-derecho sobre fila de agente |

### Accesibilidad out-of-the-box
- ARIA completo, keyboard navigation, focus management.
- Soporte screen readers validado (VoiceOver, NVDA).
- Cumple WAI-ARIA — importante si Tony exige accesibilidad.

### Breaking changes 2025
Ninguno. Los primitives unstable usan prefijo `unstable_` → opt-in.

## Fuente
- https://www.radix-ui.com/primitives
- https://www.radix-ui.com/primitives/docs/overview/releases
- https://github.com/radix-ui/primitives

## Aplicabilidad a panel-agentes
**Sí, vía shadcn/ui.** Plan:
1. `bunx shadcn@latest init` con Tailwind v4 (decidido previamente por skills-curator).
2. Agregar on-demand: `dialog`, `dropdown-menu`, `tooltip`, `tabs`, `scroll-area`, `toast`, `context-menu`.
3. No instalar Radix directo — dejar que shadcn maneje el underlying.
4. Si necesitamos un primitive sin wrapper shadcn (ej. `Slider`), instalar `@radix-ui/react-slider` directo.
