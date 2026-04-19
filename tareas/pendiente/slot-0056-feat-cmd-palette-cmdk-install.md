# Slot 0056 · feat · cmd-palette-cmdk-install

**Fase:** Fase3-Features
**Rama de habilidad:** react
**Libre para tomar:** bot con skill React + cmdk
**Asignado:** (pendiente)
**Rama git:** `slot-0056/cmdk`

## Qué hacer

Instalar `cmdk` y crear el componente base `CmdPalette.tsx` con registro de comandos extensible.

## Pasos

1. `npm install cmdk`.
2. Crear `panel-agentes/src/components/CmdPalette.tsx`.
3. Crear `panel-agentes/src/lib/cmdRegistry.ts` con tipo `Command` + funciones `register(cmd)`, `unregister(id)`, `list()`.
4. Montar `<CmdPalette />` en `App.tsx`.
5. Componente invisible por default. Se abre con estado `open: boolean` que slot-0057 (Ctrl+K) va a disparar.

## API esperada

```ts
type Command = {
  id: string;
  label: string;
  shortcut?: string;
  action: () => void | Promise<void>;
  category?: "nav" | "action" | "setting" | "help";
};

// cmdRegistry.ts
export function register(cmd: Command): void;
export function unregister(id: string): void;
export function list(): Command[];
```

## Skill base a consultar

`conocimiento/builder/2026-04-19-cmdk-command-menu.md` — ya investigada por Builder.

## AC

- [ ] `cmdk` en deps.
- [ ] `CmdPalette` componente renderiza cuando `open=true`.
- [ ] Registry permite register/unregister en tiempo de ejecución.
- [ ] Test `CmdPalette.test.tsx` con 3 casos (registrar cmd, ejecutar, desregistrar).
- [ ] Sin memory leak al unmount.

## Depende de: (ninguna — puede ir en paralelo)

## Habilita
- slot-0057 (abrir con Ctrl+K)
- slot-0058..0065 (comandos específicos)
