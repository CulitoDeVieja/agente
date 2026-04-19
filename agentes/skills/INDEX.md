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
- `railway-deploy.md` *(a crear)*
- `systemd-services.md` *(a crear)*

### Skills-Curator
- `skill-authoring.md` — cómo escribir una skill nueva.

### Architect
- `spec-authoring-AC.md` *(a crear)*
- `adr-template.md` *(a crear)*

### Scout
- `research-web.md` *(a crear)*
- `persona-journey-writing.md` *(a crear)*

## Convención de nombres
- `<categoria>-<nombre>.md` (ej: `git-workflow.md`, `nodejs-express.md`)
- Sin espacios, sin mayúsculas.

## Cómo un agente pide una skill nueva
Abre issue en `repo/orchestrator` con etiqueta `skill-request`. El Skills-Curator la toma en su próximo ciclo.
