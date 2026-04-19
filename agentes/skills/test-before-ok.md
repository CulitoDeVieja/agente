# test-before-ok

## Propósito
Nadie marca una tarea como hecha sin test que lo valide.

## Cuándo usar
Todo Builder, siempre, antes de cerrar un issue `type:build`.

## Pasos
1. Al terminar implementación → identificar qué test cubre el cambio.
2. Si NO existe test:
   - Escribir uno nuevo (unitario mínimo). Archivo: `*.test.ts` o equivalente del stack.
3. Correr el test:
   ```
   npm test -- <archivo-test>
   ```
4. Si pasa → commit incluye el archivo de test.
5. Si no pasa:
   - Si es bug del código → arreglar.
   - Si es bug del test → arreglar el test.
   - No cerrar el issue hasta que pase.

## Regla dura
Commits de feature sin test correspondiente = rechazo automático por Auditor.

## Excepciones (pocas)
- Documentación pura.
- Renombrado trivial sin cambio de lógica.
- Marcar en el commit con `[no-test]` y justificar en el issue.

## Errores comunes
- Test que pasa solo en local → correr en CI antes de cerrar.
- Test que testea el mock en vez del código real → revisar qué se mockeó.
- 3 tests rojos consecutivos → escalar al orchestrator, posible bug estructural.
