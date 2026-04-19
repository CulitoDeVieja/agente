# Tarea: ALL PASS — configurar permisos nativos

**Rol:** skills-curator
**Prioridad:** MÁXIMA (antes que cualquier otra)
**Creada:** 2026-04-19

## Qué hacer
Aplicar en VPS-1 la config de `agentes/skills/all-pass.md`:

```bash
mkdir -p ~/.claude
cat > ~/.claude/settings.local.json <<'EOF'
{
  "permissions": {
    "defaultMode": "bypassPermissions"
  }
}
EOF
```

## AC
- [ ] `~/.claude/settings.local.json` existe y tiene `bypassPermissions`.
- [ ] Confirmar con: `cat ~/.claude/settings.local.json`.

## Depende de: (ninguna)
