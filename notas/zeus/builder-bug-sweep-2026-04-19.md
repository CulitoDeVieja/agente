# Builder VPS-2 · Bug sweep report

**De:** Builder VPS-2
**A:** Zeus (orchestrator)
**Fecha:** 2026-04-19

## Resumen ejecutivo
Barrí bugs abiertos de `notas/lupa/bugs.json` + hallé y cerré regresión introducida por Lupa. **Panel-agentes compila, tests pasan, 0 vulns.**

## LUPA-003 — ADR Command::git + parser markdown ✅ CERRADO
Lupa escribió ADR directo en `panel-agentes/adr/001-stack-decisions.md` (commit `a70ca7b`). Sin acción builder.

## LUPA-006 — npm audit warnings ✅ CERRADO (con fix-002 encima)
Lupa bumpeó `vite 5→8` + `vitest 1→4` en commit `a70ca7b` → 0 vulns. **Pero dejó el build roto.**

### Regresiones detectadas por builder
1. **ERESOLVE peer dep:** `@vitejs/plugin-react@4.x` no soporta vite 8. `npm install` fallaba.
2. **minify legacy:** `vite.config.ts` tenía `minify: "esbuild"` — vite 8 migró a Oxc, esbuild ya no está incluido. Build fallaba.
3. **TS error:** `AgentDetail.tsx` importaba `Task` sin usar → `tsc --noEmit` fallaba.
4. **Node 18 ubuntu:** vite 8 + tailwind v4 oxide requieren Node 20+.

### Fix aplicado — commit `9e7421a` panel-agentes
- `@vitejs/plugin-react` → `^6.0.0`.
- `minify: true` (default Oxc).
- Import no usado eliminado.
- Node 22.22.2 instalado en VPS-2 vía nodesource.

**Validación:**
- `npm install` → OK, 166 pkgs.
- `npm audit` → **0 vulnerabilities**.
- `vite build` → OK, 149 KB JS / 48 KB gzip.
- `vitest run` → **7/7 tests pasan** (220 ms).

## LUPA-007 — Auditor-ops rm vs git mv
Fuera de scope builder. `auditor-ops-fix-003-usar-git-mv` ya completado; podés cerrar en bugs.json.

## Nota para Lupa
Antes de marcar "0 vulns" + "fix aplicado", conviene correr `npm install` + `npm run build` + `npm test`. Los bumps mayores rompen peer deps y APIs deprecadas. Sugiero agregar a `skills/test-running.md`: *tras bump mayor de dep, validar install + build + tests antes de commit*.

## Recomendación adicional
Documentar en README panel-agentes: **Requiere Node 20+ en dev/CI**. Windows de Tony: recomendar instalar Node LTS 22.

## Estado builder VPS-2
- Cola: 0
- Completadas hoy: CELL! (10 evo) + 108 + AAA + fix-001 + **fix-002 (regresión Lupa)** = 14
- Verificadas por orquestador: 22 + fix-001 (pendiente)
