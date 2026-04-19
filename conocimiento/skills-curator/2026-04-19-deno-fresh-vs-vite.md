# Deno Fresh vs Vite — 2026

## TL;DR
- **Fresh 2.0** (beta) ahora integra con Vite → 9-12x más rápido en boot. Arquitectura islands: solo manda JS al cliente para componentes interactivos.
- **Vite** sigue siendo el rey del SPA build tooling genérico (React/Vue/Svelte/Solid).
- Fresh ideal para sitios content-heavy con islands; Vite ideal para SPAs (incluye Tauri apps).

## Detalles

**Fresh 2.0 + Vite plugin:**
- Boot times 9-12x más rápidos vs Fresh 1.x.
- HMR completo + ecosystem plugins Vite.
- Routes on-demand con server code bundled.

**Islands Architecture (Fresh):**
- Server-rendering por default (JSX renderizado en servidor).
- JS cliente SOLO en componentes marcados como "island".
- Zero JS por default en páginas estáticas.

**Vite (stand-alone):**
- Build tool pure — no renderer, no router.
- Ecosystem masivo de plugins.
- Base de Tauri, Astro, Nuxt, SvelteKit.

**Diferencias prácticas:**

| | Fresh | Vite (SPA) |
|---|---|---|
| Runtime | Deno | Node / Bun |
| SSR | default | opt-in (con frameworks) |
| Islands | core feature | solo en Astro/Qwik |
| Tauri compat | ❌ (Deno server-side) | ✅ (estándar) |
| Build step | opcional (Vite plugin) | obligatorio |
| JSX framework | Preact | cualquiera |

## Fuente
- [Fresh 2.0 Graduates to Beta, Adds Vite Support - Deno Blog](https://deno.com/blog/fresh-and-vite)
- [Fresh + Vite 9-12x Faster Development - The New Stack](https://thenewstack.io/fresh-vite-means-9-12x-faster-development-for-deno/)

## Aplicabilidad a panel-agentes
**No.** Panel-agentes es app Tauri desktop, no web SSR:
1. Tauri necesita build estático para empaquetar — Fresh es server-side.
2. Fresh usa Preact, no React (dependencia distinta).
3. Deno runtime no corre en Tauri webview.

**Mantener Vite.** La skill `vite-config-tauri.md` es correcta.

**Nota futura:** si alguna vez hacemos una versión web del panel (dashboard público del estado de los agentes), **ahí** Fresh + islands tiene sentido — zero JS para páginas estáticas, JS solo para filtros interactivos. Pero no es la realidad de panel-agentes v0.1 / v0.2.
