---
title: "Deploy: <proyecto> a <entorno>"
labels: ["type:deploy"]
assignees: ["auditor-ops"]
---

## Proyecto
<nombre-repo>

## Entorno
staging / production

## Commit a deployar
Hash o tag.

## Pre-checks (Auditor los corre antes del deploy)
- [ ] Tests verdes en main
- [ ] Migraciones listas si aplica
- [ ] Variables de entorno presentes en destino

## Post-deploy
- [ ] Health check /api/health responde 200
- [ ] Smoke test manual 1 endpoint crítico

## Rollback plan
Comando exacto para revertir si falla.

## Prioridad
alta / media
