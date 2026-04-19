# Agentes

Manifiestos + skills + cola de tareas del sistema de IAs en loop. Todo vive en este repo. Sin GitHub issues, solo markdown + git.

## Estructura

```
agentes/           ← manifiestos de cada rol
  orchestrator.md
  skills-curator.md
  builder.md
  auditor-ops.md
  scout.md         (on-demand)
  architect.md     (on-demand)
  skills/          ← skills lazy-loaded
    INDEX.md
    git-workflow.md
    tareas-markdown.md
    lazy-skill-loading.md
    ...
tareas/
  pendiente/       ← tareas esperando agente
  en-curso/        ← agente trabajando
  completado/      ← terminadas
SETUP-VPS.md       ← instalar agente en VPS
```

## Cómo funciona

1. **Owner** crea archivo en `tareas/pendiente/<rol>-XXX-*.md` + commit + push.
2. **Owner dispara** `/opt/agente/ciclo.sh` en el VPS correspondiente.
3. **Agente** pull → lee manifest → toma tarea (`git mv` a `en-curso/`) → carga skills lazy → ejecuta → `git mv` a `completado/` → push.
4. Si falla → crea tarea nueva para Skills-Curator.

## Roles

| Rol | Ubicación | Loop |
|---|---|---|
| Orchestrator | local | manual |
| Skills-Curator | VPS-1 | manual |
| Builder | VPS-2 | manual |
| Auditor + Ops | VPS-3 | manual |
| Scout | sesiones on-demand | — |
| Architect | sesiones on-demand | — |

## Disciplina
- `git pull --rebase` antes de cada push.
- Test antes de marcar OK.
- No tocar código de otros agentes sin coordinar via orchestrator.
- Una skill por archivo, ≤80 líneas.
