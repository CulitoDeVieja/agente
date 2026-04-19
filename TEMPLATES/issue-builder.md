---
title: "Builder: <accion corta>"
labels: ["type:build", "in-cola"]
assignees: ["builder"]
---

## Proyecto
<nombre-repo>

## Spec
Ruta al archivo spec en el repo del proyecto. Ej: `notas/pm/paso-45-spec.md`

## Qué implementar
Descripción en 2-3 líneas. No más.

## Acceptance criteria
- [ ] AC 1 (ej: endpoint responde 200 con payload correcto)
- [ ] AC 2 (ej: test unitario cubre caso feliz + 1 edge)
- [ ] AC 3 (ej: migración DB aplica sin errores)

## Skills probables
Lista opcional de skills que el builder probablemente cargue:
- `nodejs-express.md`
- `postgres-migrations.md`

## Dependencias
- Depende del issue #XX estar cerrado antes (si aplica).
- Requiere variable de entorno `FOO_BAR` seteada.

## Prioridad
alta / media / baja
