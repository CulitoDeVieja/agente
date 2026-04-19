# Planificación

**Regla dura:** antes de que nadie programe, todo proyecto pasa por esta carpeta.

## Propósito
Forzar que estrategia, eficiencia y funcionalidad se revisen antes de tocar código. Evitar re-trabajos caros.

## Estructura por proyecto

```
planificacion/
└── <nombre-proyecto>/
    ├── 00-resumen.md          ← objetivo, scope, métricas de éxito (owner define)
    ├── 01-arquitectura.md     ← architect: stack, estructura, decisiones
    ├── 02-skills.md           ← skills-curator: qué skills hay/faltan
    ├── 03-implementacion.md   ← builder: pasos concretos (NO código, solo plan)
    ├── 04-auditoria.md        ← auditor-ops: qué se va a testear + deploy plan
    └── revision.md            ← orchestrator consolida + aprobación final
```

## Reglas

1. Cada archivo lo escribe SOLO el rol indicado.
2. Cada archivo tiene al final sección `## Estado: [borrador / en-revisión / aprobado]`.
3. Los revisores comentan al final del archivo en sección `## Comentarios de revisión`.
4. `revision.md` solo se marca ✅ cuando los 4 anteriores están aprobados.
5. Recién ahí el orchestrator crea tarea `builder-*` de programación.

## Eficiencia

- Un proyecto chico puede tener los 5 docs en <100 líneas totales.
- Si la discusión se va de 200 líneas → partir en sub-proyectos.
- Si un rol dice "no hace falta plan" → lo justifica en su archivo y el orchestrator decide.

## Flujo típico

1. Owner crea `00-resumen.md`.
2. Architect escribe `01-arquitectura.md` (on-demand).
3. Skills-curator escribe `02-skills.md` en paralelo.
4. Builder escribe `03-implementacion.md` después de leer 01 y 02.
5. Auditor-ops escribe `04-auditoria.md` leyendo todo lo anterior.
6. Orchestrator valida en `revision.md` → aprueba → crea tarea de programación.

Sin este flujo, cualquier tarea `builder-*` que llegue a pendiente se rechaza.
