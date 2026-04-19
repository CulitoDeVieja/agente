# Skills Scorecard

**Última actualización:** 2026-04-19 · arranque piloto por `lupa`
**Coordinador:** orchestrator (ver `tareas/pendiente/orchestrator-101-coordinar-scorecard-skills.md`)
**Total skills en el repo:** 115 (INDEX + 114 archivos)
**Puntuadas hasta ahora:** 6 (muestra piloto) · pendientes: 109

## Framework de puntaje

Cada skill tiene 4 dimensiones (1-5), total /20:

| Dimensión | Qué mide |
|---|---|
| Claridad | ¿Se entiende en <30s? 5=obvio / 1=confuso |
| Utilidad | ¿Resuelve problema real frecuente? 5=alto uso / 1=nunca usada |
| Madurez | ¿Propósito + cuándo + pasos + ejemplo + errores? 5=completa / 1=stub |
| Cobertura | ¿Cuántos roles la necesitan? 5=4+ roles / 1=1 rol raro |

**Bandas:** 🟢 17-20 core · 🟡 11-16 útil · 🔴 5-10 revisar · ⚫ <5 archivar

## Muestra piloto (lupa, 2026-04-19)

| Skill | Cla | Útl | Mad | Cob | Total | Banda | Nota de lupa |
|---|---|---|---|---|---|---|---|
| `git-workflow.md` | 5 | 5 | 5 | 5 | **20** | 🟢 core | 5 pasos. Sin ruido. La usa todo agente con commit. |
| `tareas-markdown.md` | 5 | 5 | 5 | 5 | **20** | 🟢 core | El corazón del sistema. Reemplaza GitHub issues. |
| `anti-choques.md` | 5 | 5 | 5 | 5 | **20** | 🟢 core | La regla de namespace por rol es la más importante del repo. |
| `lazy-skill-loading.md` | 5 | 5 | 4 | 5 | **19** | 🟢 core | Ahorra tokens. Falta ejemplo de matching con keywords reales. |
| `skill-authoring.md` | 5 | 4 | 5 | 2 | **16** | 🟡 útil | Solo skills-curator la usa, pero esencial para mantener calidad. |
| `tauri-app-architecture.md` | 4 | 5 | 5 | 2 | **16** | 🟡 útil | Core para builder del proyecto panel-agentes, inútil para el resto. Alta utilidad, baja cobertura. |

## Interpretación de lupa

**Las 4 skills base compartidas (git-workflow, tareas-markdown, anti-choques, lazy-skill-loading) son el esqueleto del sistema** — todas arriba de 19. Sin eso, el panteón no funciona.

Las skills de herramientas específicas (Tauri, Rust, React, Vite) **tienen alta utilidad pero baja cobertura** — esto va a bajar su total aunque sean imprescindibles para quien las usa. Propongo **ajustar el framework:**

> Considerar tambien una 5ª dimensión **"Criticidad para el segmento"**: si baja cobertura pero es imprescindible para el rol que la usa, puntúa 5.

O alternativamente, **normalizar**: cobertura < 3 pero utilidad = 5 → total ponderado compensa.

## Pendiente

Las 109 skills restantes serán puntuadas por **cada agente sobre su segmento**, según el plan de `orchestrator-101`. Esta muestra es referencia.

## Reglas de edición

- Solo el agente que tiene ownership del segmento actualiza su fila (anti-choques).
- Cambios al framework → requieren OK de orchestrator + Antonio.
- Cada fila lleva el campo "Nota de <agente>" con 1 frase de justificación.

---

🔍 muestra inicial de **lupa** · pido al orchestrator completar el scorecard distribuyendo sub-tareas por rol
