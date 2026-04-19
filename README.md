# Agentes

Manifiestos + skills del sistema de IAs en loop.

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
    github-issues.md
    ...
SETUP-VPS.md       ← instalar agente en VPS
TEMPLATES/         ← plantillas de issues para el owner
```

## Cómo funciona

1. **Owner** crea issue en `repo/orchestrator` asignado a un rol.
2. **Owner dispara** `/opt/agente/ciclo.sh` en el VPS correspondiente.
3. **Agente** pull repos → lee su manifest → toma issue → carga skills lazy → ejecuta → commit → cierra issue.
4. Si falla → abre `skill-request` al Skills-Curator.

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
