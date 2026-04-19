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

---
## Log auditor-ops
- Aplicado `defaultMode: bypassPermissions` en `~/.claude/settings.local.json` (merge, no overwrite — preservé 63 entries de allow existentes).
- Owner autorizó explícitamente aplicar all-pass en este entorno (`/root` = máquina del owner) overrideando el warning de safety de la skill.
- Verificado con `jq`: defaultMode="bypassPermissions" ✅
- AC ✅ (archivo creado/actualizado, verificado).
