# React Aria (Adobe) — 2026

## TL;DR
- React Aria = library de hooks + unstyled components de Adobe (React Spectrum). Foco: accesibilidad ARIA más estricta del ecosystem.
- Alternativa a Radix UI. Usada en Photoshop web, Adobe Express.
- Más verboso que Radix pero ARIA-compliant sin escapatorias.

## Detalles

**Dos APIs:**
1. **React Aria hooks** (low-level): `useButton`, `useDialog`, `useCombobox` → agregás ARIA a tu JSX.
2. **React Aria Components** (high-level): `<Button>`, `<Dialog>`, `<Combobox>` → drop-in components.

**Ejemplo hooks:**
```tsx
import { useButton } from "react-aria";

function MyButton(props) {
  const ref = useRef(null);
  const { buttonProps } = useButton(props, ref);
  return <button {...buttonProps} ref={ref}>{props.children}</button>;
}
```

**Ejemplo components:**
```tsx
import { Button, Dialog, DialogTrigger, Modal } from "react-aria-components";

<DialogTrigger>
  <Button>Open</Button>
  <Modal><Dialog>Content</Dialog></Modal>
</DialogTrigger>
```

**vs Radix UI:**

| | React Aria | Radix UI |
|---|---|---|
| A11y compliance | estricto, sin escape | serio pero con escapes |
| Verbosity | más código | más conciso |
| Compositional | hooks opcionales | Dialog.Trigger, Dialog.Content, etc. |
| Maintainer | Adobe | WorkOS / Modulz |
| Components | ~30 | ~25 |
| Docs | muy extensas | extensas |

**Cuándo React Aria:**
- Accesibilidad es prioridad #1 (gov, salud, educación).
- Equipo con experiencia a11y.
- Querés control fine-grained sobre cada interacción.

**Cuándo Radix:**
- Velocidad de desarrollo prioritaria.
- shadcn/ui como design system.
- Equipo menos experimentado en a11y detalles.

## Fuente
- [Migrating from Radix to React Aria - Argos](https://argos-ci.com/blog/react-aria-migration)
- [Headless UI alternatives - LogRocket](https://blog.logrocket.com/headless-ui-alternatives/)
- [Radix UI vs React Aria Components - Fellipe Utaka](https://www.fellipeutaka.com/blog/radix-ui-vs-react-aria-components)

## Aplicabilidad a panel-agentes
**No para v0.1 — mantener Radix/shadcn-ui.**

Razones:
1. Panel-agentes es interno (agentes + owner), no producto público con WCAG compliance obligatoria.
2. shadcn/ui + Radix cubre a11y "muy bien, no perfecto" — suficiente.
3. React Aria duplicaría trabajo ya planificado en la skill de evo 214 (shadcn-ui).

**Cuándo reconsiderar:**
- Si alguna vez hacemos un dashboard público visible a terceros (comunidad, stakeholders).
- Si agregamos accesibilidad a lectores de pantalla como req explícito.

**Nota:** shadcn-ui tiene issue abierto (#1622) sobre migrar a React Aria. Si sucede, lo heredaríamos automáticamente. Mantener atención pasiva, no acción.
