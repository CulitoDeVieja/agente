# modo-master

## Propósito
Coordinar una tarea grande entre múltiples agentes con dependencias explícitas.

## Cuándo usar
Siempre. Es skill base de todos los agentes (orchestrator, skills-curator, builder, auditor-ops, scout, architect).

## Concepto
Una tarea madre se descompone en sub-tareas, una por rol. Cada sub-tarea puede depender de otras (`## Depende de:`). El agente NO toma una sub-tarea hasta que sus dependencias estén en `tareas/completado/`.

**Regla dura:** antes de programar, pasa por `planificacion/<proyecto>/`. Ver `planificacion/README.md`. Una tarea `builder-*` NO se toma hasta que el `revision.md` del proyecto esté aprobado.

## Pasos al evaluar una tarea pendiente

1. Leer el archivo de tarea completo.
2. Buscar sección `## Depende de:`.
3. Si lista archivos → verificar cada uno en `tareas/completado/`.
4. Si **todas** las dependencias están completadas → tomar la tarea (git mv a en-curso).
5. Si **alguna** falta → skipear la tarea, mantenerla en pendiente, pasar a la siguiente.
6. Si no hay sección `## Depende de:` o dice `(ninguna)` → tomar sin restricciones.

## Ejemplo
Tarea `builder-002-implementar-panel-agentes.md`:
```
## Depende de:
- architect-001-panel-agentes-spec.md (en completado/)
- skills-curator-002-skill-tauri-architecture.md (en completado/)
```

Builder en su ciclo:
- Verifica si `tareas/completado/architect-001-panel-agentes-spec.md` existe → falta → skipea.
- Sigue buscando otra tarea `builder-*` sin dependencias bloqueadas.
- Si no hay → "sin novedades" y sale.

## Sección `## Habilita:`
Informativa. Lista qué tareas se desbloquean cuando ésta se complete. Permite que el orchestrator audite el flujo.

## Errores comunes
- Dependencia apunta a archivo que no existe en ningún estado → escalar al orchestrator (error de planificación).
- Dependencia circular (A depende de B, B de A) → escalar al orchestrator, bloqueo total.
- Archivo en `en-curso/` NO cuenta como completado. Solo `completado/`.

## Ver también
- `MODO-MASTER.md` (raíz del repo) para la visión completa.
