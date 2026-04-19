# Slot 0066 · feat · settings-modal-radix

**Fase:** Fase3-Features
**Rama de habilidad:** react
**Libre para tomar:** bot con skill React + Radix UI
**Asignado:** (pendiente)
**Rama git:** `slot-0066/settings-modal`

## Qué hacer

Crear el modal de Settings con `@radix-ui/react-dialog`. Estructura vacía con tabs; los controles se cargan en slots 0067-0075.

## Pasos

1. `npm install @radix-ui/react-dialog @radix-ui/react-tabs`.
2. Crear `panel-agentes/src/components/SettingsModal.tsx`.
3. Prop: `open: boolean` + `onOpenChange: (open: boolean) => void`.
4. Tabs: `General · Tema · Atajos · Avanzado`.
5. Trigger: botón en statusbar + comando en CmdPalette.
6. Cierre: Esc + click backdrop + botón X.

## Layout

```
┌──────────── Settings ────────────[X]┐
│ [General] Tema  Atajos  Avanzado    │
├─────────────────────────────────────┤
│                                     │
│ (contenido de tab activa)          │
│                                     │
├─────────────────────────────────────┤
│              [Cancelar] [Guardar]  │
└─────────────────────────────────────┘
```

## Skill base a consultar

`conocimiento/builder/2026-04-19-radix-ui-primitives-v2.md`

## AC

- [ ] Modal abre/cierra con prop.
- [ ] Tabs funcionales (click cambia contenido).
- [ ] Esc cierra.
- [ ] Backdrop click cierra.
- [ ] Focus trap dentro del modal.
- [ ] Test `SettingsModal.test.tsx` con ≥4 casos.

## Depende de:
- `slot-0016-ui-dashboard-4-cards-grid.md` (para App estructurado)

## Habilita
- slot-0067 (theme radio)
- slot-0068..0075 (controles específicos)
