# Tarea: Preparar repo `panel-agentes` en GitHub

**Rol:** auditor-ops (modo ops)
**Prioridad:** media
**Creada:** 2026-04-19
**Modo master:** panel-agentes — preparación infra

## Qué hacer
Dejar el repo listo para que el builder clone y arranque. SIN código todavía — solo estructura base.

## Pasos
1. Confirmar con owner si `gh auth` está activo en VPS-3 (si no, escalar).
2. `gh repo create CulitoDeVieja/panel-agentes --public --clone` en VPS-3.
3. Crear `README.md` mínimo con descripción del proyecto.
4. Crear `.gitignore` para Tauri + Node.
5. Primer commit + push.
6. Reportar URL del repo creado al orchestrator vía log.

## Acceptance criteria
- [x] Repo `CulitoDeVieja/panel-agentes` existe en GitHub.
- [x] Tiene README mínimo.
- [x] Tiene `.gitignore` apropiado.
- [x] URL del repo en el log de esta tarea.

## Depende de:
(ninguna — puede arrancar ya; si gh auth falta, escalar y bloquear)

## Habilita:
- builder-002-implementar-panel-agentes (necesita repo para clonar)

---

## Log del agente
- Repo creado: https://github.com/CulitoDeVieja/panel-agentes (público, creado 2026-04-19)
- README.md y .gitignore (Tauri + Node) pusheados en branch `ops/init`
  - Último commit en ops/init: ver `gh api repos/CulitoDeVieja/panel-agentes/commits/ops/init --jq .sha`
- **Decisión pendiente del owner:** política de este repo bloquea push directo a `main`. Opciones:
  1. Owner renombra/promueve `ops/init` a default branch (`gh api -X PATCH repos/CulitoDeVieja/panel-agentes -f default_branch=ops/init`).
  2. Owner aprueba un PR `ops/init → main` (requiere crear `main` primero manualmente).
- Visibility: `public`. Si owner quiere privado: `gh repo edit CulitoDeVieja/panel-agentes --visibility private --accept-visibility-change-consequences`.
- Builder puede clonar ya desde `ops/init` para arrancar bootstrap (paso 1 de builder).
