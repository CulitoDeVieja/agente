# skill-authoring

## Propósito
Que cada skill sea útil, corta y reusable. Sin esto, el repo de skills se hace ruido.

## Cuándo usar
Skills-Curator, cada vez que crea o edita una skill.

## Formato obligatorio
```markdown
# <nombre-en-minusculas>

## Propósito
Una línea: qué problema resuelve.

## Cuándo usar
Cuándo un agente debe invocar esta skill.

## Pasos
Numerados, accionables. Sin ambigüedad.

## Ejemplo
Caso concreto con código si aplica.

## Errores comunes
2-4 trampas que ya vimos, con cómo salir.
```

## Reglas
- Máx 80 líneas. Si más, partir en 2 skills.
- Un concepto por skill. No mezclar.
- Sin introducciones filosóficas. Directo al grano.
- Código en bloques `bash` / `ts` / `sql`.

## Cuándo NO crear skill nueva
- Si existe una con 80% del contenido → mejor editar la existente.
- Si es one-off (no se va a reusar) → no vale la pena.
- Si depende de contexto muy específico de un proyecto → guardar en el proyecto, no en `repo/agentes`.

## Proceso al terminar
1. Agregar entrada en `skills/INDEX.md` bajo la categoría correcta.
2. Commit `skills: add <nombre-skill>`.
3. Si reemplaza una vieja → borrar vieja y notar en commit.

## Errores comunes
- Skill que explica teoría en vez de pasos → reescribir accionable.
- Skill con 10 opciones → elegir la recomendada, mencionar alternativas en 1 línea.
- Duplicados con nombres distintos → Skills-Curator debe detectar y consolidar mensualmente.
