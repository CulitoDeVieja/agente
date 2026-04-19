# Tarea: ALL PASS — configurar permisos nativos

**Rol:** auditor-ops
**Prioridad:** MÁXIMA
**Creada:** 2026-04-19

## Qué hacer
Aplicar en VPS-3 la config de `agentes/skills/all-pass.md`:

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
- [ ] Archivo creado.
- [ ] Verificado con cat.

## Depende de: (ninguna)
