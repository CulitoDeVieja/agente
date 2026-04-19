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
- [ ] Repo `CulitoDeVieja/panel-agentes` existe en GitHub.
- [ ] Tiene README mínimo.
- [ ] Tiene `.gitignore` apropiado.
- [ ] URL del repo en el log de esta tarea.

## Depende de:
(ninguna — puede arrancar ya; si gh auth falta, escalar y bloquear)

## Habilita:
- builder-002-implementar-panel-agentes (necesita repo para clonar)

---

## Log del agente
(vacío hasta completar)
