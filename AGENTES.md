# AGENTES — identificación en el repo

Cuando consultes el repo, primero identificá quién sos según tu variable `$AGENT_ROLE` o el prompt que te llegó. Tus tareas siempre llevan tu prefijo.

---

## Los 7 roles

| # | Nombre | Prefijo tareas | Host | Modo | Qué hace |
|---|---|---|---|---|---|
| **0** | **Orchestrator** (también Zeus) | `orchestrator-*` | local del owner | loop 1min | Coordina, consolida, revisa PRs, escala al owner. Único que responde directo al owner. |
| **1** | **Skills-Curator** | `skills-curator-*` | VPS-1 (Hostinger) | loop 1min | Crea/cura/mejora skills en `agentes/skills/`. |
| **2** | **Builder** | `builder-*` | VPS-2 (Hostinger) | loop 1min | Programa código real (React + TS + Rust Tauri). |
| **3** | **Auditor-Ops** | `auditor-ops-*` / `ops-*` | VPS-3 (Hostinger) | loop 1min | QA dinámica + deploy + release + infra. |
| **4** | **Lupa** | `lupa-*` | Claude Code session | on-demand | Inspector profundo + tester. Mantiene `notas/lupa/bugs.json`. |
| **5** | **Estado** | `estado-*` | Claude Code session | on-demand | Dashboard STATE.md + snapshots JSON para la app. |
| **6** | **Tester** | `tester-*` | Claude Code session | on-demand | Suites unit + integration + E2E Playwright + smoke tests Windows. |

Agentes on-demand (#4/#5/#6) **ejecutan todas sus tareas en una sola sesión** — el owner los dispara manualmente, no hay loop. Deben tomar todas las tareas de su prefijo y procesarlas de un tirón.

## Cómo identificarte al iniciar un ciclo

1. `echo $AGENT_ROLE` — debe devolver tu rol exacto (ej: `builder`).
2. Leer tu manifest: `agentes/<tu-rol>.md`.
3. Buscar tareas: `ls tareas/pendiente/ | grep ^<tu-rol>-`.
4. Tomar primera sin dependencias bloqueadas (ver skill `modo-master.md`).

## Ramas git por agente (v0.2)

Cada agente trabaja en su branch del repo `panel-agentes`:

```
main ← protegida, solo orchestrator mergea
├── agent-1/skills        (skills-curator)
├── agent-2/builder       (builder)
├── agent-3/auditor-ops   (auditor-ops)
├── agent-4/lupa          (lupa)
├── agent-5/estado        (estado)
└── agent-6/tester        (tester)
```

Nunca pushees a `main` directo. Siempre a tu branch + PR.

## Reglas comunes

- **Namespace:** solo tocá archivos con tu prefijo. `STATE.md` e `INDEX.md` son excepciones compartidas — ver `anti-choques.md` R2.
- **Un commit por tarea** (R5 anti-choques).
- **`git pull --rebase` antes de cada push.**
- **Test antes de OK** si tu tarea tiene código.
- **Silencio > ruido** si un ciclo no tiene aporte real.

## Si no sabés quién sos

Si arrancás un ciclo y `$AGENT_ROLE` no está definido:
1. Detenete.
2. Abrí issue a orchestrator creando `tareas/pendiente/orchestrator-quien-soy-<timestamp>.md`.
3. Esperá aclaración.

**Nunca asumas un rol que no se te asignó.**
