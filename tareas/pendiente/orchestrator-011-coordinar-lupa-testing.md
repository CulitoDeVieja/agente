# Tarea: Coordinar Lupa para testing final

**Rol:** orchestrator (dual Lupa)
**Prioridad:** alta
**Creada:** 2026-04-19

## Qué hacer
Preparar el gate de testing final liderado por Lupa. Pasos:

1. Crear `planificacion/panel-agentes/test-plan.md` con:
   - Lista de suites a correr (vitest + cargo test + E2E Playwright futuro).
   - Métricas target (bundle <15MB, cold start <2s).
   - Matriz de escenarios manuales.
2. Cuando builder-002 + builder-108..219 estén todas completas Y auditor-ops haga build Windows:
   - Lupa corre el test-plan completo.
   - Genera `notas/lupa/test-report-v0.1.md` con hallazgos.
   - Por cada bug → tarea `builder-fix-*`.
3. Gate: si 0 bugs críticos → luz verde para release v0.1.0.

## AC
- [ ] `test-plan.md` creado.
- [ ] Lupa (yo, dual) ejecuta plan cuando condiciones estén dadas.
- [ ] Test report publicado.
- [ ] Gate decidido (release / fix loop).

## Depende de:
- `builder-002-implementar-panel-agentes` (en completado ✅)
- Build Windows `.msi` disponible (pendiente — requiere VPS Windows)

## Habilita
- Release v0.1.0 público.
