# Tarea: Implementar app panel-agentes (Tauri)

**Rol:** builder
**Prioridad:** alta
**Creada:** 2026-04-19
**Modo master:** panel-agentes
**Proyecto:** panel-agentes (repo nuevo)

## Qué hacer
Implementar la app según spec de architect. Stack: Tauri 2 + React + TS + Tailwind.

Features mínimos:
- Dashboard con 4 cards (1 por agente en loop).
- Click en card → vista detalle con pendientes / en-curso / últimas 10 completadas.
- Botón refrescar → `git pull` del repo local.
- Build Windows `.msi` + `.exe` portable.

## Acceptance criteria
- [ ] Repo `panel-agentes` creado con estructura Tauri estándar.
- [ ] UI corre en `npm run tauri dev`.
- [ ] Lee directo del filesystem `C:/Users/Tony/AppData/Local/Temp/agente-repo/`.
- [ ] Build genera `.msi` de <15 MB.
- [ ] Tests unitarios de las funciones de parseo de tareas (≥3 tests).

## Depende de:
- `architect-001-panel-agentes-spec.md` (en completado/)
- `skills-curator-002-skill-tauri-architecture.md` (en completado/)

## Habilita:
- auditor-ops-002-audit-panel-agentes.md

---

## Log del agente
(vacío hasta completar)
