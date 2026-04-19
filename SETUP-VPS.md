# SETUP-VPS — Instalación de agente en VPS Hostinger

Pasos para loguear Claude Code y dejar el agente listo para que el owner loopee manual.

Tiempo estimado: **15 min por VPS**.

## 0. Pre-requisitos

- VPS Ubuntu 22.04+ con acceso SSH.
- Cuenta Claude con plan Pro Max activo.
- Cuenta GitHub con acceso al repo `orchestrator` y al repo del proyecto.

## 1. Instalar Claude Code

```bash
# Instalar Node.js LTS
curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo -E bash -
sudo apt-get install -y nodejs

# Instalar Claude Code CLI
npm install -g @anthropic-ai/claude-code

# Verificar
claude --version
```

## 2. Login interactivo

```bash
claude /login
```

Seguir el flujo OAuth (copia-pega token del navegador). Queda guardado en `~/.claude/`.

## 3. Clonar repos

```bash
sudo mkdir -p /opt/agente
sudo chown $USER:$USER /opt/agente
cd /opt/agente

git clone https://github.com/CulitoDeVieja/agente.git
# Clonar los repos de proyecto que el agente vaya a tocar:
# git clone https://github.com/<owner>/<proyecto>.git
```

Configurar git identity (el rol firma commits):

```bash
git config --global user.name "<ROL> (agente)"
git config --global user.email "<rol>@agente.local"
```

## 4. Definir rol del agente

```bash
echo "export AGENT_ROLE=builder" >> ~/.bashrc
# Reemplazar "builder" por: skills-curator / auditor-ops / orchestrator
source ~/.bashrc
```

## 5. Script de ciclo (1 archivo)

Crear `/opt/agente/ciclo.sh`:

```bash
#!/bin/bash
set -e
cd /opt/agente/agente
git pull --rebase

# Arranca sesión de Claude Code con contexto del rol
claude --prompt "Sos el agente $AGENT_ROLE. Lee /opt/agente/agente/agentes/$AGENT_ROLE.md y seguí sus instrucciones. Tus skills base están en /opt/agente/agente/agentes/skills/. Tu cola: ls /opt/agente/agente/tareas/pendiente/ | grep ^$AGENT_ROLE-. Si hay tarea: git mv a tareas/en-curso, ejecutá, git mv a tareas/completado con log. Si cola vacía → 'sin novedades' y salí."
```

```bash
chmod +x /opt/agente/ciclo.sh
```

## 6. Loopear manual (owner dispara)

Cada vez que quieras un turno del agente:

```bash
ssh vps-X
/opt/agente/ciclo.sh
```

O por cola: crear archivos `tareas/pendiente/<rol>-XXX-*.md` en el repo `agente`, commit + push, después disparar el ciclo N veces en el VPS del rol correspondiente.

## 7. Verificación

```bash
# Rol correcto
echo $AGENT_ROLE

# Repos presentes
ls /opt/agente/

# Claude autenticado
claude whoami
```

---

## Por VPS

| VPS | AGENT_ROLE | Repos extras que necesita clonar |
|---|---|---|
| Local | `orchestrator` | todos los de proyectos |
| VPS-1 | `skills-curator` | solo `agente` |
| VPS-2 | `builder` | `agente` + proyectos activos |
| VPS-3 | `auditor-ops` | `agente` + proyectos activos |

## Troubleshooting

- `claude: command not found` → npm global path. Agregar `export PATH=$PATH:$(npm prefix -g)/bin` al `.bashrc`.
- Agente no encuentra tareas → verificar que `$AGENT_ROLE` está exportado + que hay archivos con ese prefijo en `tareas/pendiente/`.
- `git pull` falla → conflicto de merge. Otro agente pushó en paralelo. Rebase manual una vez, después reintenta.
