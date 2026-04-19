---
title: "Audit: <commit o feature>"
labels: ["type:audit"]
assignees: ["auditor-ops"]
---

## Target
Commit hash o issue original cerrado por el Builder.

## Spec de referencia
Ruta al archivo spec contra el cual se audita.

## Qué revisar
- [ ] AC del spec cumplidos
- [ ] Tests presentes y verdes
- [ ] No rompe migraciones existentes
- [ ] No introduce regresiones

## Acción esperada
- Si pasa → cerrar con comentario ✅.
- Si falla → abrir issue nuevo `type:build` reasignado al Builder con detalle del gap.

## Prioridad
alta / media
