# Slot 0106 · feat · theme-context-provider

**Fase:** Fase3-Features
**Rama de habilidad:** react
**Libre para tomar:** bot con skill React Context
**Asignado:** (pendiente)
**Rama git:** `slot-0106/theme-context`

## Qué hacer

Context de tema (light/dark/system) con persistencia en localStorage. Base para todos los slots de theming (0107-0113).

## Pasos

1. Crear `panel-agentes/src/context/ThemeContext.tsx`.
2. Estado: `"light" | "dark" | "system"` con default `"system"`.
3. Detectar preferencia OS con `matchMedia("(prefers-color-scheme: dark)")`.
4. Aplicar clase `dark` en `<html>` según resolución.
5. Persistir en `localStorage["panel-agentes-theme"]`.
6. Hook `useTheme()` expone: `{ theme, setTheme, resolvedTheme }`.

## API

```ts
type Theme = "light" | "dark" | "system";

type ThemeContextValue = {
  theme: Theme;
  setTheme: (t: Theme) => void;
  resolvedTheme: "light" | "dark";
};

export function ThemeProvider({ children }: { children: ReactNode }): JSX.Element;
export function useTheme(): ThemeContextValue;
```

## Requisitos

- Listener a cambios de `prefers-color-scheme` (dispara re-render si `theme === "system"`).
- SSR-safe (pero acá es Vite CSR, trivial).
- Transición suave al cambiar tema (opcional, css).

## AC

- [ ] Provider + hook expuestos.
- [ ] Persiste en localStorage.
- [ ] Detecta cambio de sistema.
- [ ] Aplica clase `dark` en html cuando corresponde.
- [ ] Test `ThemeContext.test.tsx` con ≥4 casos (default, toggle, persist, system change).

## Depende de: (ninguna)

## Habilita
- slot-0107 (system detection)
- slot-0108 (toggle component)
- slot-0109..0113 (theme features)
