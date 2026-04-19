# Tarea: Coordinar scorecard de skills + regenerar STATE.md

**Rol:** orchestrator
**Prioridad:** media (documental, no bloqueante)
**Creada:** 2026-04-19 por `lupa`
**Origen:** pedido de Antonio a lupa: *"anda revisando y verificando las nuevas habilidades y explicalas simple para mi. iras puntuando las habilidades en una especie de puntaje para ir midiendo las nuevas habilidades y lo utiles que son. cada agente sube las actualizaciones de su segmento"*

## Contexto

`agentes/skills/` tiene **115 skills** (INDEX + 114 archivos .md). Antonio quiere un sistema de puntaje para medir cuáles son útiles y cuáles no, y que **cada agente score sus propias skills** (las que usa en su rol).

Lupa NO lo ejecuta porque sería imponer decisión sobre territorio que es de cada agente especializado. **Orchestrator coordina.**

## Framework de puntaje propuesto por lupa

Cada skill se puntúa en **4 dimensiones (1-5)** = total /20:

| Dimensión | Qué mide | Cómo puntuar |
|---|---|---|
| **Claridad** | ¿Se entiende en <30 segundos? | 5 = obvio al mirar / 1 = hay que leer 3 veces |
| **Utilidad** | ¿Resuelve problema real frecuente? | 5 = se usa en 80% de los ciclos / 1 = nunca se usó |
| **Madurez** | ¿Tiene propósito + cuándo usar + pasos + ejemplo + errores? | 5 = los 5 secciones / 1 = solo el título |
| **Cobertura de rol** | ¿Cuántos roles la necesitan? | 5 = 4+ roles / 1 = solo 1 rol en casos raros |

**Puntaje total:**
- 17-20: 🟢 core (mantener y propagar)
- 11-16: 🟡 útil (aceptable, mejorar si queda tiempo)
- 5-10: 🔴 revisar (quizás fusionar, reescribir o archivar)
- <5: ⚫ archivar (a `agentes/skills/archivadas/`)

## Proceso de scoring distribuido

Cada agente puntúa las skills **de su segmento**. Responsabilidades:

| Agente | Skills a puntuar | Archivos |
|---|---|---|
| **builder** | Tauri + Rust + Vite + build | `tauri-*.md`, `vite-*.md`, `rust-*.md`, `worktree-isolation.md`, `test-before-ok.md` |
| **auditor-ops** | tests, deploy, systemd, CI | `test-running.md`, `ci-*.md`, `systemd-*.md`, `railway-*.md`, `code-coverage-*.md` |
| **skills-curator** | meta-skills (escribir skills, INDEX) | `skill-authoring.md`, `lazy-skill-loading.md`, `INDEX.md` |
| **architect** | specs, ADRs | `spec-authoring-AC.md`, `adr-template.md` |
| **scout** | research, user journeys | `research-web.md`, `persona-journey-writing.md` |
| **orchestrator** | git-workflow, tareas-markdown, anti-choques, modo-master | base skills compartidas |

Las skills **compartidas** (usadas por 2+ roles) se puntúan en paralelo, se hace promedio.

## Output esperado

Archivo `agentes/SKILLS-SCORECARD.md` con formato:

```markdown
# Skills Scorecard — 2026-04-19

| Skill | Claridad | Utilidad | Madurez | Cobertura | Total | Banda | Rol scoreó |
|---|---|---|---|---|---|---|---|
| git-workflow.md | 5 | 5 | 5 | 5 | 20 | 🟢 core | orchestrator |
| tauri-app-architecture.md | 4 | 5 | 5 | 2 | 16 | 🟡 | builder |
| ... | | | | | | | |
```

Skills < 10 van a `agentes/skills/archivadas/` (movimiento físico con log explícito).

## Proceso paso a paso

1. **Orchestrator** crea 6 sub-tareas en `tareas/pendiente/`:
   - `builder-210-scorecard-segmento-builder.md`
   - `auditor-ops-110-scorecard-segmento-auditor.md`
   - `skills-curator-220-scorecard-segmento-curator.md`
   - `architect-030-scorecard-segmento-architect.md`
   - `scout-010-scorecard-segmento-scout.md`
   - `orchestrator-102-scorecard-segmento-base.md`
2. Cada agente, al tomar su sub-tarea, usa el framework (4 dimensiones × 1-5) y escribe su sección del scorecard.
3. **Orchestrator consolida** en `agentes/SKILLS-SCORECARD.md` cuando las 6 estén completas.
4. **Orchestrator decide** qué archivar (< 10 puntos) con OK de Antonio.

## Nota lateral: STATE.md desactualizado otra vez

Lupa detecta que **STATE.md sigue con números viejos** (24 pend skills-curator, 100 auditor-ops) mientras el filesystem real tiene:
- auditor-ops: 11 pend, builder: 15 pend, skills-curator: 14 pend (al 2026-04-19T04:30)

Esto indica que `tools/health.sh` (pedido en `orchestrator-100`) **aún no fue implementado**. Prioridad: alta. Sin health check automático, STATE.md se va a seguir desviando.

## Depende de:
`orchestrator-100-regenerar-estado-real.md` (el guard anti-false-complete + health script). Si ese no se hace, el scorecard está sobre datos podridos.

## Habilita:
- Decisión de Antonio sobre qué skills vale la pena mantener
- Ahorro de tokens (lazy loading no carga skills inútiles)
- Base para que futuros agentes nuevos sepan qué usar

## Log del agente
(vacío hasta completar)
