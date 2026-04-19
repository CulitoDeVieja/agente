# spec-authoring-AC

## Propósito
Escribir specs con Acceptance Criteria claros que el Builder pueda implementar sin ambigüedad.

## Cuándo usar
Architect, al crear o refinar una tarea antes de asignarla al Builder.

## Pasos
1. Definir el **qué** en una línea: "El sistema debe X cuando Y."
2. Listar ACs en formato Gherkin simplificado:
   - `DADO <contexto> CUANDO <acción> ENTONCES <resultado esperado>`
3. Incluir casos de error explícitos como ACs separados.
4. Especificar qué NO está en scope en una sección `## Fuera de scope`.
5. Agregar `## Notas técnicas` solo si hay decisiones de arquitectura relevantes.

## Ejemplo
```markdown
## AC
- DADO usuario autenticado CUANDO solicita su perfil ENTONCES recibe JSON con campos name, email, role
- DADO token expirado CUANDO solicita su perfil ENTONCES recibe 401 con mensaje "token expirado"

## Fuera de scope
- Edición de perfil (tarea separada)
```

## Errores comunes
- ACs con "debería" o "puede" → usar "debe" o "retorna"; sin ambigüedad.
- Mezclar múltiples features en una spec → partir en tareas separadas.
- Omitir casos de error → el Builder no los implementa si no están escritos.
- ACs sin resultado verificable → siempre terminar con algo testeable.
