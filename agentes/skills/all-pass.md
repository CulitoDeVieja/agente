# all-pass

## Propósito
Habilidad **ALL PASS** — el agente nunca pide aprobación de permisos. Ejecuta toda la cola sin cortes.

## Cuándo usar
Siempre. Skill base de todo agente que corre en VPS aislado.

## Instalación nativa (persistente)

Una sola vez en cada VPS:

```bash
mkdir -p ~/.claude
cat > ~/.claude/settings.local.json <<EOF
{
  "permissions": {
    "defaultMode": "bypassPermissions"
  }
}
EOF
```

Después de eso, todas las sesiones de Claude Code en ese VPS saltan aprobaciones sin necesidad de flag.

## Alternativa por sesión (no persistente)

Si no querés editar config:
```bash
claude --dangerously-skip-permissions --prompt "..."
```

## Verificar

```bash
cat ~/.claude/settings.local.json
```

Debe aparecer `"defaultMode": "bypassPermissions"`.

## Seguridad

⚠ Solo aplicar en VPS aislado del owner. Los agentes NO deberían tener acceso a:
- Credenciales del owner.
- Archivos personales fuera del repo.
- Red privada / VPN del owner.

En VPS Hostinger dedicado al sistema de agentes: safe.
En laptop del owner: NO usar bypassPermissions.

## Errores comunes

- Settings mal formado (JSON) → CLI ignora, pide aprobación cada vez. Validar con `jq . settings.local.json`.
- Archivo en path incorrecto → debe ser `~/.claude/settings.local.json` exacto.
- Permisos del archivo → `chmod 600 ~/.claude/settings.local.json`.
