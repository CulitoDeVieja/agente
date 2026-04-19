# Spec panel-agentes — plan ejecutable para Builder

**Autor:** architect (psique)
**Tarea madre:** architect-001-panel-agentes-spec
**Fecha:** 2026-04-19

**Nota:** el detalle de stack, componentes, comandos Tauri y tipos vive en `planificacion/panel-agentes/01-arquitectura.md` (Fase A del modo master). Este documento es el plan de implementación para Builder.

---

## Plan de 3-5 pasos para Builder

### Paso 1 — Bootstrap del repo `panel-agentes`

- `cargo create-tauri-app panel-agentes` (template React + TS + Vite).
- `git init`, primer push a `CulitoDeVieja/panel-agentes`.
- Agregar Tailwind v4 (`@tailwindcss/vite`) al `vite.config.ts`.
- Agregar deps de `package.json` y `Cargo.toml` listadas en `01-arquitectura.md §5`.
- `cargo tauri dev` abre ventana en blanco sin errores.

**Criterio:** `cargo tauri dev` arranca; `cargo tauri build` genera `.msi` y portable `.exe`.

---

### Paso 2 — Comandos Tauri + parser de tareas

- `src-tauri/src/commands.rs`: implementar las 6 signatures (ver `01-arquitectura.md §3`).
- `src-tauri/src/parse.rs`: parser markdown (ver tabla de headings en `§6`).
- Test Rust: `cargo test` con 3 fixtures (`.md`) en `src-tauri/tests/fixtures/`.
- `git2` para `git_pull` — abrir repo, fetch remote `origin`, `reset --hard` a `origin/HEAD`.
- Config persistence: `%APPDATA%/panel-agentes/config.json` via `tauri::api::path`.

**Criterio:** los 6 comandos responden en REPL Tauri con datos del repo agente local.

---

### Paso 3 — UI Dashboard

- `<ConfigProvider>` + `<RepoDataProvider>` (React Context, ver ADR-001).
- `<Header>` con `<ModoMasterBadge>` + `<RefreshButton>` + botón `⚙` config.
- `<Dashboard>` con 4 `<AgentCard>` (grid 2×2 en <1000px, 1×4 en >=1000px).
- Card muestra: rol, estado (✅/⏳), conteos pendiente/en-curso/completado, última señal.
- Al arrancar: si `repoPath` inválido → modal config; si válido → fetch inicial (sin `git_pull`).

**Criterio:** abrir app con repo agente clonado muestra los 4 cards con datos reales de `STATE.md` + conteos.

---

### Paso 4 — Vista detalle + routing

- Hash routing simple: `useRoute()` lee `location.hash`.
- `#/agent/:role` → `<AgentDetail>`.
- `<TaskList>` por estado, último completado limit 10.
- `<BackLink>` vuelve a `#/`.
- Log de tarea en-curso se muestra colapsado (expand on click).

**Criterio:** click en card → vista detalle con las 3 listas; back link vuelve; navegación con `history.back()` funciona.

---

### Paso 5 — Refresh + polish

- Botón ↻ dispara `api.gitPull()` + re-fetch; loader spinning; toast rojo si falla.
- (opcional) `autoRefreshOnFocus`: listener `window.focus` → refresh.
- Empty states: "(ninguna)" cuando lista vacía.
- Dark mode fijo (Tailwind `@theme` con paleta oscura minimal).
- Build final: `cargo tauri build --target x86_64-pc-windows-msvc`.
- Release `v0.1.0` con `.msi` + portable `.exe` adjuntos.

**Criterio:** métricas del `00-resumen.md`: `.msi <15MB`, app abre <2s, refresh <1s, cero credenciales hardcodeadas.

---

## Wireframes (ASCII)

### Dashboard
```
┌───────────────────────────────────────────────────────────┐
│ PANEL AGENTES                  [MODO MASTER ●]  [⚙] [↻]   │
├───────────────────────────────────────────────────────────┤
│  ┌───────────┐  ┌───────────┐  ┌───────────┐  ┌─────────┐ │
│  │ ORCH      │  │ SKILLS    │  │ BUILDER   │  │ AUDITOR │ │
│  │ ✅        │  │ ⏳        │  │ ✅        │  │ ⏳      │ │
│  │ P: 0      │  │ P: 2      │  │ P: 1      │  │ P: 2    │ │
│  │ E: 1      │  │ E: 0      │  │ E: 0      │  │ E: 1    │ │
│  │ ✓: 0      │  │ C: 0      │  │ C: 1      │  │ C: 0    │ │
│  │ (hace 3m) │  │ (hace 5m) │  │ (hace 3m) │  │ (hace 8m)│ │
│  └───────────┘  └───────────┘  └───────────┘  └─────────┘ │
└───────────────────────────────────────────────────────────┘
```

### Detalle
```
┌───────────────────────────────────────────────────────────┐
│ ← Volver                BUILDER (VPS-2) ✅                │
├───────────────────────────────────────────────────────────┤
│ PENDIENTES (2)                                            │
│   ○ builder-002-implementar-panel  [alta] 🔒 bloqueada   │
│      └─ depende: architect-001, skills-curator-002        │
│   ○ builder-003-llenar-03-implementacion  [alta]         │
├───────────────────────────────────────────────────────────┤
│ EN CURSO (0)                                              │
│   (ninguna)                                               │
├───────────────────────────────────────────────────────────┤
│ COMPLETADAS (últimas 10)                                  │
│   ✓ builder-001-reportar-senal     2026-04-19 13:28      │
└───────────────────────────────────────────────────────────┘
```

---

## Nota sobre el repo `panel-agentes`

El repo `CulitoDeVieja/panel-agentes` fue creado **público** por error en esta sesión (política pide privado). Antes de que Builder empiece el Paso 1:
1. Owner (Antonio) decide: privado (default recomendado) o público.
2. Si privado: `gh repo edit CulitoDeVieja/panel-agentes --visibility private`.
3. Builder clona y arranca.
