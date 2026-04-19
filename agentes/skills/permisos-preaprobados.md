# permisos-preaprobados

## Propósito
Que los agentes en VPS ejecuten toda la cola de tareas **sin interrupciones** por aprobación manual de tools.

## Cuándo usar
Al instalar/configurar cualquier agente en VPS. Skill base operativa.

## Opción A — Modo "aceptar todo" (recomendado en VPS aislado)

Script del ciclo:
```bash
claude --dangerously-skip-permissions --prompt "..."
```

⚠ **Solo en VPS aislado del owner.** Aprobación omitida total. Apto porque:
- El VPS solo tiene acceso al repo git + filesystem propio.
- No hay datos sensibles del owner.
- Cada acción queda trazada en commits.

## Opción B — Whitelist por tool (más conservadora)

Archivo `~/.claude/settings.json`:
```json
{
  "permissions": {
    "allow": [
      "Bash(git:*)",
      "Bash(npm:*)",
      "Bash(cargo:*)",
      "Bash(ls:*)",
      "Bash(cat:*)",
      "Bash(mkdir:*)",
      "Bash(mv:*)",
      "Bash(cp:*)",
      "Bash(gh:*)",
      "Read(*)",
      "Write(*)",
      "Edit(*)",
      "Glob(*)",
      "Grep(*)"
    ]
  }
}
```

Cualquier tool fuera de la lista → bloqueado.

## Configuración por agente

Cada agente del panel tiene su whitelist específica en el mismo `settings.json`:

| Agente | Tools permitidos |
|---|---|
| orchestrator | git, read/write/edit .md, gh |
| skills-curator | git, write .md en agentes/skills/ |
| builder | git, npm, cargo, tauri, write código |
| auditor-ops | git, npm test, cargo test, gh release, lighthouse |

## Verificar al iniciar ciclo

Antes de entrar al loop:
```bash
claude --help | grep -q "dangerously-skip-permissions" || echo "WARN: version vieja de Claude CLI"
ls ~/.claude/settings.json || echo "WARN: settings.json no existe"
```

## Errores comunes
- Agente se corta a mitad de tarea → permisos no pre-aprobados. Solución: aplicar esta skill.
- `settings.json` mal formado → CLI ignora la config, pide aprobación por cada tool. Validar JSON.
- Whitelist demasiado restrictiva → tarea falla por tool ausente. Revisar logs y agregar.
