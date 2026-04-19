# Índice de Skills

Cada skill es un archivo corto (≤80 líneas) con: propósito · cuándo usar · pasos · ejemplo · errores comunes.

Se cargan **lazy**: el agente lee solo las que necesita para la tarea actual, para ahorrar tokens.

## Skills base (todos los agentes las cargan siempre)

- `git-workflow.md` — pull --rebase, push, resolución de conflictos.
- `tareas-markdown.md` — tomar, trabajar y completar tareas desde `tareas/`.
- `lazy-skill-loading.md` — cómo decidir qué skills cargar.
- `modo-master.md` — respetar dependencias entre tareas encadenadas.
- `anti-choques.md` — namespace por rol + retry con backoff.
- `all-pass.md` — bypass de prompts de permiso en VPS aislados.
- `evolucion.md` — investigar web y guardar conocimiento por rol.
- `fase-b-ejecucion.md` — reglas de ejecución post-planificación.
- `permisos-preaprobados.md` — allowlist de comandos seguros.

## Skills por rol

### Builder
- `worktree-isolation.md` — trabajar en worktree propio, no branch.
- `test-before-ok.md` — obligatorio test antes de cerrar issue.

### Auditor + Ops
- `test-rust-cargo.md` — correr test suites Rust con cargo.
- `react-testing-vitest.md` — correr tests JS/TS con Vitest.
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

## Skills avanzadas Tauri
- `tauri-notifications.md` — notificaciones nativas del sistema.
- `tauri-system-tray.md` — ícono y menú en bandeja del sistema.
- `tauri-auto-start.md` — arranque automático al iniciar Windows.
- `tauri-window-management.md` — gestionar ventanas (crear, ocultar, posicionar).
- `tauri-menu-native.md` — menú nativo de aplicación.
- `tauri-shell-open.md` — abrir URLs y archivos en el browser/explorer nativo.
- `tauri-global-shortcut.md` — atajos globales de teclado fuera de foco.
- `tauri-v2-breaking-changes.md` — cambios breaking de Tauri v1 a v2.
- `tauri-permissions-config.md` — permisos granulares con capabilities JSON.
- `tauri-csp-hardening.md` — hardening de Content Security Policy.
- `websockets-rust-tauri.md` — WebSockets en Rust integrado con Tauri 2.
- `sqlite-in-tauri.md` — SQLite en Tauri 2 con rusqlite.

## Skills React avanzadas
- `react-context-splitting.md` — dividir Context para evitar re-renders.
- `react-query-basics.md` — fetching y caching con TanStack Query.
- `react-virtual-list.md` — listas largas con virtualización.
- `react-dnd-kit.md` — drag and drop con @dnd-kit/core.
- `react-hot-toast.md` — notificaciones toast con react-hot-toast.
- `react-hotkeys-hook.md` — atajos de teclado con react-hotkeys-hook.
- `tanstack-query-setup.md` — configurar TanStack Query v5.
- `optimistic-updates.md` — updates optimistas en UI.
- `debounce-throttle.md` — debounce y throttle de eventos.
- `memoize-heavy-computations.md` — memoizar cálculos costosos.
- `timer-cleanup-react.md` — limpiar timers en useEffect.
- `abort-signal-fetch.md` — cancelar requests con AbortController.
- `react-suspense-basics.md` — lazy loading (ya listado arriba).
- `virtualization-tasks-list.md` — virtualizar lista de tareas con @tanstack/virtual.
- `compose-panel-layout.md` — composición de layout de paneles.
- `undo-redo-pattern.md` — undo/redo con useReducer.
- `state-persistence.md` — persistir estado entre sesiones.

## Skills Tailwind avanzadas
- `tailwind-custom-theme.md` — extender tema con colores y fuentes custom.
- `tailwind-plugins.md` — usar y crear plugins de Tailwind.
- `tailwind-animations.md` — animaciones CSS con Tailwind.
- `tailwind-typography-plugin.md` — tipografía rich text con @tailwindcss/typography.

## Skills TypeScript avanzadas
- `typescript-generics.md` — genéricos para funciones y tipos reutilizables.
- `typescript-utility-types.md` — Partial, Pick, Omit, Record, ReturnType.
- `zod-schema-validation.md` — validación con Zod.
- `yup-vs-zod.md` — decisión Yup vs Zod.
- `fetch-vs-axios.md` — decisión fetch vs axios.

## Skills Rust avanzadas
- `rust-tokio-async.md` — programación async con Tokio.
- `rust-error-thiserror.md` — manejo de errores con thiserror.
- `rust-tracing.md` — logging estructurado con tracing.
- `github-api-rust.md` — consumir API de GitHub desde Rust.
- `rust-panic-recovery.md` — capturar y recuperarse de panics.
- `json-file-storage.md` — persistir config y estado en archivos JSON.

## Skills Testing / CI / Release
- `test-e2e-playwright.md` — tests E2E con Playwright.
- `test-rust-cargo.md` — tests unitarios en Rust con cargo test.
- `ci-github-actions.md` — CI/CD con GitHub Actions para Tauri + React.
- `release-please.md` — automatizar releases con release-please.
- `semantic-release.md` — releases automáticos con semantic-release.
- `changelog-generation.md` — generar CHANGELOG desde commits.
- `code-coverage-istanbul.md` — cobertura con Istanbul/nyc.
- `vitest-coverage.md` — cobertura con Vitest + @vitest/coverage-v8.
- `bundle-analyzer.md` — analizar tamaño del bundle con rollup-plugin-visualizer.
- `lighthouse-desktop.md` — auditar performance de apps desktop.

## Skills UX / Accesibilidad / Copy
- `accessibility-axe.md` — auditar accesibilidad con axe-core.
- `contrast-checker.md` — verificar contraste WCAG.
- `accessibility-keyboard.md` — navegación por teclado accesible.
- `screen-reader-aria.md` — ARIA roles y labels para lectores de pantalla.
- `error-message-copy.md` — redactar mensajes de error útiles.
- `empty-state-copy.md` — redactar estados vacíos que guíen al usuario.
- `loading-state-copy.md` — redactar mensajes de carga.
- `success-feedback-copy.md` — redactar feedback de éxito.
- `notification-levels.md` — niveles info/warning/error/success.
- `confirm-destructive-action.md` — confirmación antes de acciones destructivas.
- `figma-to-react.md` — convertir diseños Figma a React + Tailwind.
- `storybook-basics.md` — documentar componentes con Storybook.
- `i18n-react-intl.md` — internacionalización con react-intl.
- `render-markdown-safely.md` — renderizar markdown con sanitización XSS.

## Skills Datos / Análitica
- `polling-vs-push.md` — decisión polling vs WebSockets/SSE.
- `local-cache-invalidation.md` — estrategias de invalidación de cache local.
- `error-tracking-sentry.md` — capturar errores en producción con Sentry.
- `analytics-privacy-first.md` — analytics con Plausible/Umami.
- `logging-winston.md` — logging con Winston (Node.js).
- `logging-pino.md` — logging de alta performance con Pino.

## Convención de nombres
- `<categoria>-<nombre>.md` (ej: `git-workflow.md`, `nodejs-express.md`)
- Sin espacios, sin mayúsculas.

## Cómo un agente pide una skill nueva
Crea archivo `tareas/pendiente/skills-curator-XXX-nueva-skill-<nombre>.md` describiendo el gap. El Skills-Curator la toma en su próximo ciclo.
