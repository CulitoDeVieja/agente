# Tarea: Regenerar STATE.md del filesystem real + reglas anti-false-complete

**Rol:** orchestrator
**Prioridad:** alta (bloqueante)
**Creada:** 2026-04-19 por `lupa`
**Origen:** fix de 204 tareas false-completed, ver `notas/FIX-FALSE-COMPLETED-20260419.md`

## Contexto

Lupa detectó y revirtió 204 tareas en `completado/` cuyo `## Log del agente` estaba vacío. Las regresó a `pendiente/`. Ahora los agentes de VPS las pueden picar de verdad.

## Qué hacer

### 1. Actualizar STATE.md con números reales

Tabla que debe reflejar exactamente el filesystem:

```
| Rol            | Pendiente | En-curso | Completado |
|----------------|-----------|----------|------------|
| architect      | 0         | 0        | 72         |
| auditor-ops    | 1         | 0        | 99         |
| builder        | 99        | 0        | 2          |
| orchestrator   | 4         | 1        | 0          |
| skills-curator | 100       | 0        | 1          |
```

Script para regenerar automáticamente (pegar en `tools/health.sh`):

```bash
#!/bin/bash
cd $(dirname $0)/..
for rol in architect auditor-ops builder orchestrator skills-curator; do
    p=$(ls tareas/pendiente/ 2>/dev/null | grep -c "^$rol-")
    e=$(ls tareas/en-curso/ 2>/dev/null | grep -c "^$rol-")
    c=$(ls tareas/completado/ 2>/dev/null | grep -c "^$rol-")
    echo "| $rol | $p | $e | $c |"
done
```

### 2. Agregar guard en `ciclo.sh` de cada VPS

Antes de `git mv` a `completado/`, chequear que el log del agente NO esté vacío:

```bash
if grep -q "(vacío hasta completar)" "$TASK_FILE" || \
   ! grep -q "^## Log del agente" "$TASK_FILE"; then
    echo "ERROR: tarea $TASK_FILE sin log real. No se marca done."
    exit 1
fi
```

Aplicar en `SETUP-VPS.md` paso 5.

### 3. Regla de ownership

Un agente con `AGENT_ROLE=X` sólo puede `git mv` archivos `X-NNN-*.md`. Guard:

```bash
BASENAME=$(basename "$TASK_FILE")
if [[ ! "$BASENAME" =~ ^${AGENT_ROLE}- ]]; then
    echo "ERROR: no es tu rol. Tarea pertenece a: ${BASENAME%%-*}"
    exit 1
fi
```

### 4. By-pass del owner con evidencia

Cuando Antonio hace "Fase aprobada by-pass deps", antes del `git mv` debe:
1. Abrir el archivo
2. Agregar bajo `## Log del agente`: `By-pass owner — <fecha> — razón: <breve>`
3. Commit con `[owner-bypass] …`

Así los reportes quedan trazables.

## Acceptance criteria

- [ ] STATE.md actualizado con tabla de números reales (de los 5 roles)
- [ ] `tools/health.sh` creado y ejecutable, cuenta real-time desde filesystem
- [ ] `SETUP-VPS.md` actualizado con guard de log vacío + guard de ownership en `ciclo.sh`
- [ ] `notas/FIX-FALSE-COMPLETED-20260419.md` linkeado desde STATE.md como "Incidente 2026-04-19"
- [ ] Si alguno de los 204 archivos revertidos contradice decisión de Antonio (ej: él sí quiso hacer by-pass formal), re-moverlos a `completado/` con log explícito

## Depende de:
(ninguna — arranca ya)

## Habilita:
- skills-curator puede empezar a picar sus 100 tareas reales
- builder puede continuar sus 99 restantes
- auditor-ops puede cerrar su tarea pendiente

## Log del agente
(vacío hasta completar)
