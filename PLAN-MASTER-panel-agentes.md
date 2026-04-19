# Plan Master — Panel de Control de Agentes

**Modo master:** activo
**Creado por:** orchestrator
**Fecha:** 2026-04-19

---

## Objetivo

App nativa Windows (`.exe`) que lee el repo `agente` y muestra en vivo el progreso de cada agente: qué tareas tiene asignadas, cuál está en curso y cuáles completó.

## Stack (definido por orchestrator)

| Pieza | Elección | Por qué |
|---|---|---|
| Framework | **Tauri 2** | Exe <10MB, nativo Windows, seguro |
| Frontend | **React 18 + TypeScript + Vite** | Rápido, tipado, DX conocido |
| Estilos | **Tailwind CSS** | Sin CSS a mano |
| Backend embebido | **Rust** | Ya viene con Tauri, accede a filesystem + git |
| Build | **`cargo tauri build`** → `.msi` + `.exe` portable | Un comando |

No hay servidor HTTP. La app lee directo del filesystem local (`C:/Users/Tony/AppData/Local/Temp/agente-repo/`).

---

## Features v0.1

1. **Dashboard** — 4 cards (orchestrator / skills-curator / builder / auditor-ops). Cada card:
   - Rol + estado (desde `STATE.md`)
   - Conteo tareas: `pendiente` / `en-curso` / `completado`
   - Última fecha de actividad
2. **Vista detalle por agente** (click en card):
   - Tareas pendientes (título + prioridad + dependencias bloqueantes)
   - Tarea en-curso (título + log parcial si existe)
   - Últimas 10 completadas (título + timestamp)
3. **Refrescar** — botón que corre `git pull --rebase` y recarga.
4. **Indicador de modo master** — si hay `## MODO MASTER activo` en `STATE.md`, header muestra badge.

## Features v0.2 (fuera de alcance v0.1)

- Crear tarea desde UI (form → archivo).
- Stream de commits (git log en vivo).
- Disparar ciclo de agente remoto (SSH).

---

## Estructura del repo `panel-agentes` (a crear)

```
panel-agentes/
├── src-tauri/                  ← backend Rust
│   ├── src/
│   │   ├── main.rs
│   │   └── commands.rs         ← read_tareas, read_state, git_pull
│   ├── tauri.conf.json
│   └── Cargo.toml
├── src/                        ← frontend React
│   ├── main.tsx
│   ├── App.tsx
│   ├── components/
│   │   ├── AgentCard.tsx
│   │   ├── AgentDetail.tsx
│   │   ├── TaskList.tsx
│   │   └── RefreshButton.tsx
│   ├── lib/
│   │   ├── tauri.ts            ← invocar comandos Rust
│   │   └── parseTask.ts        ← parseo markdown → objeto
│   └── types.ts
├── public/
├── index.html
├── package.json
├── tsconfig.json
├── vite.config.ts
└── README.md
```

---

## Comandos Tauri (Rust → JS)

```rust
#[tauri::command]
fn list_tasks(estado: &str, rol: &str) -> Vec<Task> { ... }
// estado: "pendiente" | "en-curso" | "completado"
// rol: "orchestrator" | "builder" | ... | "all"

#[tauri::command]
fn read_state() -> StateSnapshot { ... }
// parsea STATE.md

#[tauri::command]
fn read_task(archivo: &str) -> TaskDetail { ... }
// lee markdown, extrae secciones

#[tauri::command]
fn git_pull() -> GitResult { ... }
// corre git pull --rebase en el repo
```

## Tipos TS (frontend)

```ts
type AgentRole = "orchestrator" | "skills-curator" | "builder" | "auditor-ops";

type Task = {
  id: string;           // "builder-002-implementar-panel"
  role: AgentRole;
  title: string;
  priority: "alta" | "media" | "baja";
  estado: "pendiente" | "en-curso" | "completado";
  dependeDe: string[];  // archivos de tareas
  file: string;         // ruta relativa
};

type AgentSnapshot = {
  role: AgentRole;
  ubicacion: string;
  ultimaSenal: string;
  pendientes: number;
  enCurso: number;
  completadas: number;
};
```

---

## Wireframe — Dashboard

```
┌───────────────────────────────────────────────────────────┐
│ PANEL AGENTES                  [MODO MASTER ●]  [↻]      │
├───────────────────────────────────────────────────────────┤
│                                                           │
│  ┌───────────┐  ┌───────────┐  ┌───────────┐  ┌─────────┐│
│  │ ORCH      │  │ SKILLS    │  │ BUILDER   │  │ AUDITOR ││
│  │ ✅ activo │  │ ⏳        │  │ ✅ activo │  │ ⏳      ││
│  │ P: 0      │  │ P: 2      │  │ P: 1      │  │ P: 2    ││
│  │ C: 1      │  │ E: 0      │  │ E: 0      │  │ E: 1    ││
│  │ ✓: 0      │  │ C: 0      │  │ C: 1      │  │ C: 0    ││
│  └───────────┘  └───────────┘  └───────────┘  └─────────┘│
└───────────────────────────────────────────────────────────┘
```

## Wireframe — Detalle

```
┌───────────────────────────────────────────────────────────┐
│ ← Volver                BUILDER (VPS-2) ✅               │
├───────────────────────────────────────────────────────────┤
│ PENDIENTES (2)                                            │
│   ○ builder-002-implementar-panel  [alta] 🔒 bloqueada   │
├───────────────────────────────────────────────────────────┤
│ EN CURSO (0)                                              │
│   (ninguna)                                               │
├───────────────────────────────────────────────────────────┤
│ COMPLETADAS (últimas 10)                                  │
│   ✓ builder-001-reportar-senal    2026-04-19             │
└───────────────────────────────────────────────────────────┘
```

---

## Flujo de ejecución (MODO MASTER)

```
PARALELO (arrancan ya):
  ├─ architect    → architect-001-panel-agentes-spec
  │                 escribe spec detallada en notas/architect/
  └─ skills-curator → skills-curator-002-skill-tauri-architecture
                      skill `tauri-app-architecture.md` en agentes/skills/

↓ ambas completadas ↓

SECUENCIAL:
  builder → builder-002-implementar-panel-agentes
            crea repo panel-agentes, implementa v0.1, tests

↓ completada ↓

  auditor-ops → auditor-ops-002-audit-panel-agentes
                corre tests, build .msi/.exe, publica release v0.1.0
```

## Puntos de sincronización

| Hito | Qué reporta | Actualiza en | Orchestrator hace |
|---|---|---|---|
| H1 | architect-001 en completado/ | STATE.md | Marca sub-tarea ✅ |
| H2 | skills-curator-002 en completado/ | INDEX.md + STATE.md | Marca sub-tarea ✅ |
| H3 | builder-002 en completado/ | STATE.md | Marca ✅, libera auditor-ops-002 |
| H4 | auditor-ops-002 en completado/ | STATE.md | Modo master termina, reporta owner |

## Criterio de "hecho"

Todo H1-H4 en verde + `.msi` presente en `panel-agentes/releases/v0.1.0/` + la app abre y muestra las 4 cards con datos reales del repo.

---

*orchestrator — el plan manda; los roles ejecutan.*
