# React Server Components en app desktop (Tauri/Electron)

## TL;DR
- **RSC no encaja nativamente en Tauri/Electron**: requieren un servidor Node (Next.js app router) que una app desktop no quiere embeber.
- Workaround Next.js: usar **Static Site Generation (SSG/`output: "export"`)** o páginas puramente cliente; perdés RSC.
- Alternativa que da "sabor RSC" sin server: **React 19 Compiler + Suspense + `use()`** sobre comandos Tauri.

## Detalles

### Por qué falla RSC en desktop
- RSC renderiza en Node y streamea payload serializado al cliente → necesita proceso server vivo.
- Tauri es single-binary con WebView; Electron puede embeber Node pero duplica memoria y anula el motivo de usar RSC (offload de bundle).
- Bridges tipo `next-electron-server` existen pero son frágiles en prod Windows (code-signing, permisos).

### Qué SÍ funciona en Tauri 2
1. **Next.js estático**: `next.config.js` con `output: "export"` → HTML+JS listos, sin server. Perdés RSC, quedás con Client Components.
2. **Vite + React 19**: más limpio para Tauri; React Compiler automemoiza igual que RSC reduce re-renders.
3. **Patrón "async data via invoke()"**:
   ```tsx
   const agents = use(invoke<Agent[]>('list_agents'))  // React 19 `use()` + Suspense
   ```
   Obtenés UX tipo RSC (loading boundaries, data-fetching declarativo) sin server JS.

### Cuándo valdría la pena forzar RSC
- App con SEO (no aplica a desktop).
- Equipo con codebase Next.js grande que quiere portar rápido → usar SSG export.
- En cualquier otro caso: **Vite + React 19 + Tauri** es mejor DX y bundle menor.

## Fuente
- https://dev.to/ottoaria/tauri-in-2026-build-cross-platform-desktop-apps-with-web-technologies-better-than-electron-11mo
- https://tech-insider.org/tauri-vs-electron-2026/
- https://blog.nishikanta.in/tauri-vs-electron-the-complete-developers-guide-2026

## Aplicabilidad a panel-agentes
**No adoptar RSC.** Panel-agentes es Tauri + Vite (ya decidido en `revision.md`). Si queremos la ergonomía "fetch declarativo" de RSC, usar React 19 `use()` + Suspense sobre `invoke()` de Tauri — mismo patrón sin necesidad de server Node. Data server-side real va en Rust vía commands tipados (TauRPC).
