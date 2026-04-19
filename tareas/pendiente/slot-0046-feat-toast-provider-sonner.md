# Slot 0046 · feat · toast-provider-sonner

**Fase:** Fase3-Features
**Rama de habilidad:** react
**Libre para tomar:** bot con skill React + Sonner
**Asignado:** (pendiente)
**Rama git:** `slot-0046/sonner-toast`

## Qué hacer

Instalar y configurar `sonner` como sistema de toasts del panel. Wrapper global + helper functions.

## Pasos

1. `npm install sonner`.
2. Crear `panel-agentes/src/lib/toast.ts` exportando helpers: `toast.success(msg)`, `toast.error(msg, { retry? })`, `toast.info(msg)`.
3. Agregar `<Toaster />` en `App.tsx` con config: top-right, tema según app theme, max 3 simultáneos.
4. Integrar con dark mode (prop `theme`).

## Skill base a consultar

`conocimiento/builder/2026-04-19-sonner-toasts.md` — ya investigada por #2 Builder. Contiene ejemplo de setup.

## AC

- [ ] `sonner` en `package.json` dependencies.
- [ ] `src/lib/toast.ts` exporta 3+ helpers typed.
- [ ] `<Toaster />` en App con theme-aware.
- [ ] Test `toast.test.ts` con ≥2 casos (success mock, error mock).
- [ ] No rompe bundle size target (<200KB gzip).

## Depende de:
- `slot-0016-ui-dashboard-4-cards-grid.md` (para tener App estructurado)

## Habilita
- slot-0047..0055 (todas las tareas de toasts específicos)
- slot-0056..0065 (error handlers que disparan toasts)
