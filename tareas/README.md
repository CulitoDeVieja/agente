# Cola de Tareas

Sistema de coordinación basado en markdown + git. No usa GitHub issues.

## Estructura

```
tareas/
├── pendiente/      ← tareas esperando agente
├── en-curso/       ← agente tomó y está trabajando
└── completado/     ← terminadas (archivo histórico)
```

## Convención de nombre de archivo

```
<rol>-<id>-<titulo-corto>.md
```

Ejemplos:
- `builder-001-implementar-checkout.md`
- `skills-curator-002-nueva-skill-react.md`
- `auditor-003-audit-paso-45.md`

## Ciclo

1. **Owner** crea archivo en `tareas/pendiente/<rol>-XXX-*.md`.
2. **Agente** en su loop:
   - `git pull --rebase`
   - Lista `tareas/pendiente/<su-rol>-*.md`
   - Toma el primero → `git mv` a `tareas/en-curso/`
   - `git push` (marca que está trabajando)
   - Ejecuta la tarea
   - `git mv` a `tareas/completado/` con comentario al final del archivo
   - `git push`
3. Si cola vacía → "sin novedades" y sale.

## Formato del archivo de tarea

```markdown
# <titulo>

**Rol:** builder
**Prioridad:** alta
**Creada:** 2026-04-19
**Proyecto:** mercado

## Qué hacer
Descripción en 2-3 líneas.

## Acceptance criteria
- [ ] AC 1
- [ ] AC 2

## Skills probables
- skills/nodejs-express.md
- skills/mercadopago-api.md

## Dependencias
Ninguna / depende de tarea #XXX

---

## Log del agente (se llena al completar)
(vacío hasta que el agente termine)
```

## Anti-choques

- Cada tarea apunta a un archivo o carpeta específica del proyecto.
- Si 2 tareas tocan el mismo archivo → el owner las ordena secuencial, no paralelo.
- Mientras hay un `<rol>-*` en `en-curso/`, ningún otro archivo del mismo rol se toma.
