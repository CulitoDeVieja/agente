# fase-b-ejecucion

## Propósito
Reglas para la fase de programación real del panel-agentes. Skill base activa durante Fase B.

## Cuándo usar
Cuando un agente ejecuta una tarea que **escribe código real** (no markdown de planificación).

## Flujo obligatorio por tarea de código

1. **Pull antes:** `git pull --rebase` en el repo del proyecto + en `agente`.
2. **Leer plan:** abrir `planificacion/panel-agentes/detalle/<rol>-<tema>.md` del árbol propio.
3. **Codear** en worktree propio, no en `main` directo.
4. **Test local:** correr test correspondiente.
   - Si NO hay test → escribir uno mínimo antes de cerrar (regla `test-before-ok.md`).
   - Si pasa → seguir.
   - Si falla → arreglar antes de commit.
5. **Commit + push** inmediato (regla `anti-choques.md` R5). Un commit por tarea.
6. **Informar al orchestrator** agregando línea al final de la tarea en `tareas/completado/`:
   ```
   ## Log del agente
   - Commit: <hash>
   - Tests: ✅ | ❌
   - Si ❌: descripción del error + archivo/línea
   ```
7. **Mover a completado** con `git mv`. Push.

## Si se detecta error (auditor-ops)

El auditor corre tests en cada push del builder:
1. Si test falla → crear tarea nueva `builder-fix-<num>-<nombre>.md` con:
   - Commit que introdujo el error.
   - Logs del test fallido.
   - Archivo/línea sugerida.
2. Mover la tarea original a `completado/` con `Tests: ❌` + linkear a la tarea de fix.
3. Builder toma la tarea de fix en su próximo ciclo.

## Rol del orchestrator durante Fase B

- **Cada ciclo:** detectar tareas recién completadas en `tareas/completado/` → consolidar en `STATE.md` + verificar que el flujo test/error funciona.
- **Unificar:** cuando múltiples agentes completan piezas relacionadas, orchestrator escribe nota en `notas/orchestrator/union-<feature>.md` marcando qué ya está integrado.
- **Conflictos:** si 2 commits tocan el mismo archivo → orchestrator decide merge + resuelve.

## Código perfecto = sincronía
- Nadie sigue programando si un test está rojo.
- El error se fixea con prioridad alta ANTES de tomar tareas nuevas.
- Si un agente detecta que su código depende de otro agente que aún no terminó → pausa + crea tarea de coordinación al orchestrator.

## Errores comunes
- Hacer commit sin correr test → auditor lo rechaza, re-abre tarea.
- Saltarse el pull antes → conflicto de merge, rebase doloroso.
- Fixear sin entender el error → re-introduce el bug. Leer los logs completos primero.
