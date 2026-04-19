# Índice de Skills

Cada skill es un archivo corto (≤80 líneas) con: propósito · cuándo usar · pasos · ejemplo · errores comunes.

Se cargan **lazy**: el agente lee solo las que necesita para la tarea actual, para ahorrar tokens.

## Skills base (todos los agentes las cargan siempre)

- `git-workflow.md` — pull --rebase, push, resolución de conflictos.
- `tareas-markdown.md` — tomar, trabajar y completar tareas desde `tareas/`.
- `lazy-skill-loading.md` — cómo decidir qué skills cargar.
- `modo-master.md` — respetar dependencias entre tareas encadenadas.
- `anti-choques.md` — namespace por rol + retry con backoff.

## Skills por rol

### Builder
- `worktree-isolation.md` — trabajar en worktree propio, no branch.
- `test-before-ok.md` — obligatorio test antes de cerrar issue.

### Auditor + Ops
- `test-running.md` — correr test suites.
- `railway-deploy.md` — deployar servicios a Railway desde CLI.
- `systemd-services.md` — gestionar servicios systemd en VPS.

### Skills-Curator
- `skill-authoring.md` — cómo escribir una skill nueva.

### Architect
- `spec-authoring-AC.md` — escribir specs con Acceptance Criteria accionables.
- `adr-template.md` — documentar decisiones de arquitectura.

### Scout
- `research-web.md` — investigar tecnologías y comparar opciones.
- `persona-journey-writing.md` — escribir personas y user journeys.

## Skills panel-agentes (Tauri + React + TS)

### Tauri / Rust
- `tauri-app-architecture.md` — estructura proyecto Tauri 2 + React + TS.
- `tauri-rust-commands-ipc.md` — exponer funciones Rust al frontend via IPC.
- `tauri-filesystem-access.md` — leer y listar archivos con permisos correctos.
- `tauri-run-cli-command.md` — ejecutar comandos shell desde Rust.
- `tauri-windows-build.md` — generar .msi y .exe portable para Windows.
- `tauri-updater-future.md` — referencia auto-updater para versiones futuras.
- `vite-config-tauri.md` — configurar Vite para desarrollo y build con Tauri.
- `rust-serde-json.md` — serializar/deserializar structs Rust ↔ JSON.

### React / Frontend
- `react-tailwind-setup.md` — configurar Tailwind CSS v4 en Vite + React.
- `react-router-setup.md` — navegación multi-vista con React Router v6.
- `react-testing-vitest.md` — tests de componentes con Vitest + Testing Library.
- `react-hooks-patterns.md` — patrones de hooks para estado async.
- `react-suspense-basics.md` — lazy loading y Suspense para componentes.
- `error-boundary-pattern.md` — capturar errores de render con Error Boundaries.
- `zustand-vs-context-decision.md` — elegir entre Zustand y React Context.
- `tailwind-dark-mode.md` — tema oscuro con clase `dark` en Tailwind v4.

### TypeScript / Tooling
- `typescript-strict-mode.md` — activar strict mode y resolver errores comunes.
- `eslint-prettier-setup.md` — configurar ESLint + Prettier en proyecto TS.
- `code-splitting-vite.md` — dividir bundle en chunks para mejor performance.

### Datos / Utils
- `markdown-parsing.md` — parsear markdown con YAML frontmatter desde Rust o TS.
- `date-fns-usage.md` — formatear y manipular fechas con date-fns v3.

### Ops / Release
- `github-releases-workflow.md` — crear releases con assets en GitHub via CLI.
- `semver-conventions.md` — convenciones de versionado semántico.
- `logging-strategy.md` — logging en Rust y React para debugging efectivo.

## Convención de nombres
- `<categoria>-<nombre>.md` (ej: `git-workflow.md`, `nodejs-express.md`)
- Sin espacios, sin mayúsculas.

## Cómo un agente pide una skill nueva
Abre issue en `repo/orchestrator` con etiqueta `skill-request`. El Skills-Curator la toma en su próximo ciclo.
