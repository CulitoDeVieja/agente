# Índice de Skills

Cada skill es un archivo corto (≤80 líneas) con: propósito · cuándo usar · pasos · ejemplo · errores comunes.

Se cargan **lazy**: el agente lee solo las que necesita para la tarea actual, para ahorrar tokens.

## Skills base (todos los agentes las cargan siempre)

- `git-workflow.md` — pull --rebase, push, resolución de conflictos.
- `tareas-markdown.md` — tomar, trabajar y completar tareas desde `tareas/`.
- `lazy-skill-loading.md` — cómo decidir qué skills cargar.

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

## Convención de nombres
- `<categoria>-<nombre>.md` (ej: `git-workflow.md`, `nodejs-express.md`)
- Sin espacios, sin mayúsculas.

## Cómo un agente pide una skill nueva
Abre issue en `repo/orchestrator` con etiqueta `skill-request`. El Skills-Curator la toma en su próximo ciclo.
