# Vite + Rolldown — estado actual (abril 2026)

## TL;DR
- Vite 6 quedó atrás: **Vite 8 estable (marzo 2026)** reemplaza esbuild+rollup por **Rolldown** (bundler único en Rust) — builds 10-30x más rápidos.
- Para proyectos en Vite 7 hay paquete puente **`rolldown-vite`**: aísla issues Rolldown antes de saltar a v8.
- Compat layer auto-convierte `esbuild`/`rollupOptions` → equivalentes Rolldown.

## Detalles

### Migración recomendada
```bash
# Opción A: directo a Vite 8
bun add -D vite@^8

# Opción B: intermedio (proyectos complejos)
bun add -D rolldown-vite  # = Vite 7 con Rolldown, sin otras rupturas
# Validar builds, luego subir a vite@8
```

### Ganancias reales
- Cold start dev: similar a Vite 5/6 (esbuild ya era rápido en dev).
- **Build prod**: 10-30x en repos medianos (medido por VoidZero en Vue/Nuxt).
- Menos RAM en CI: un solo pipeline en Rust vs dos en JS.

### Posibles rupturas
- Plugins rollup antiguos que usan APIs deprecadas → `rollupOptions.plugins` filtra.
- Custom `esbuild` transforms: revisar, suelen migrarse con shim.
- Source maps: formato equivalente, chequear si herramienta downstream depende de field específico.

### Compatibilidad Tauri
Tauri 2 + Vite 8 funciona out-of-the-box; `src-tauri/tauri.conf.json` → `build.devUrl` y `build.beforeBuildCommand` sin cambios.

## Fuente
- https://vite.dev/blog/announcing-vite8
- https://voidzero.dev/posts/announcing-rolldown-vite
- https://byteiota.com/vite-8-0-rolldown-migration-guide-10-30x-faster-builds/

## Aplicabilidad a panel-agentes
**Sí.** El proyecto aún no arrancó con bundler fijo. Decisión: **empezar directo en Vite 8** (estable desde marzo 2026). Ganás builds prod 10-30x más rápidos y evitás migración futura. Plan: `bun add -D vite@^8`, `@vitejs/plugin-react@latest`, `@tauri-apps/cli@^2`. Si aparece plugin incompatible, caer a `rolldown-vite` como fallback durante sprint 1.
