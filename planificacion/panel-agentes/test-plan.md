# Test Plan — panel-agentes v0.1

**Responsable:** Lupa (dual con orchestrator)
**Fecha:** 2026-04-19
**Gate:** 0 bugs críticos → release v0.1.0

---

## 1. Suites automáticas

### 1.1 Unit (vitest · frontend)
- `parseTask` — parseo markdown con frontmatter (3+ casos: válido, sin frontmatter, inválido).
- `parseTaskList` — iteración + filtrado por rol.
- Hooks (useRepoData, useAgents cuando existan).

Comando: `cd panel-agentes && npm test`
Target: **100% verde**, ≥6 tests.

### 1.2 Unit (cargo test · backend Rust)
- Comandos Tauri (list_tasks, read_state, read_task, git_pull).
- Parser markdown en Rust.

Comando: `cd panel-agentes/src-tauri && cargo test`
Target: **100% verde**.

### 1.3 E2E (Playwright — futuro v0.2)
Skipeado en v0.1. Tarea para builder: setup Playwright en ronda siguiente.

---

## 2. Métricas target

| Métrica | Target | Crítico |
|---|---|---|
| Bundle `.msi` | <15 MB | >25 MB |
| Cold start | <2 s | >5 s |
| RAM idle | <150 MB | >400 MB |
| CPU idle | <2% | >10% |
| Refresh click → UI actualizada | <1 s | >3 s |

Herramientas: `dust` (tamaño), `Get-Process` Windows (RAM/CPU), timestamps manuales.

---

## 3. Escenarios manuales

1. **Arranque:** doble clic `.msi` → app abre sin errores → 4 cards visibles.
2. **Detalle:** click card Builder → lista completadas/pendientes visible.
3. **Refresh:** cambiar algo en el repo local → click ↻ → UI refleja el cambio.
4. **Modo master:** badge "MODO MASTER" visible cuando corresponde.
5. **Notificaciones:** al menos 1 notif visible si hay tareas bloqueadas.
6. **Cierre:** X → app cierra sin crash, reopen → abre en último estado visible.
7. **Offline:** desconectar red → no crashea, muestra indicador offline.

Cada escenario: ✅ pasa / ❌ falla con screenshot + pasos.

---

## 4. Flujo de reporte de bugs

Cada bug detectado por Lupa crea tarea:

```
builder-fix-<NNN>-<bug-slug>.md
```

Contenido:
- **Título:** resumen de 1 línea.
- **Severidad:** crítica / alta / media / baja.
- **Commit culpable:** `git bisect` si aplica.
- **Repro:** pasos numerados.
- **Screenshot:** link o adjunto.
- **Logs:** último stack trace relevante.
- **Fix esperado:** orientación sin código.

---

## 5. Gate de release

| Severidad | Cantidad permitida |
|---|---|
| Crítica | **0** |
| Alta | 0 (recomendado) / 1 con workaround (aceptable) |
| Media | ≤3 |
| Baja | ≤10 |

Si el gate se cumple → Lupa firma `GATE-PASS-v0.1.md` → auditor-ops sube release.

---

## 6. Cuándo ejecuto yo (Lupa)

- Trigger 1: `builder-002` + builds Windows completados.
- Trigger 2: cada Pull Request mayor al repo `panel-agentes`.
- Trigger 3: on-demand del owner.

## 7. Pendiente v0.1

Requiere `.msi` en Windows. Hasta entonces, solo puedo:
- Validar tests unitarios vitest / cargo.
- Revisar código estático (con auditor-ops).
- **No puedo correr escenarios manuales ni medir métricas sin el binario.**

Cuando haya `.msi` → Lupa ejecuta test-plan completo en 1 sesión.
