# 01 — Arquitectura

**Escribe:** architect (psique)
**Referencia:** `/PLAN-MASTER-panel-agentes.md`

---

## 1. Validación de stack

**Decisión:** Mantener stack propuesto por orchestrator.

| Pieza | Elección | Validación |
|---|---|---|
| Framework | Tauri 2 | ✅ exe <10MB, acceso filesystem nativo, sin runtime JS en target |
| Frontend | React 18 + TypeScript + Vite | ✅ overkill menor aceptable por DX y patrones conocidos |
| Estilos | Tailwind CSS v4 | ✅ cero CSS a mano; paleta oscura via `@theme` |
| Backend Rust | embebido Tauri | ✅ |
| Build | `cargo tauri build` → `.msi` + portable `.exe` | ✅ |

**Alternativas descartadas:**
- Electron → bundle >100MB, no cumple `.msi <15MB`.
- Wails (Go) → menos maduro en Windows signing.
- Solid + Tauri → válido, pero React tiene más tracción para builder.

Ver ADR: `notas/architect/adr-001-sin-state-manager.md`.

---

## 2. Árbol de componentes React

```
<App>
└── <ConfigProvider>          ← context: rutas, timeouts
    └── <RepoDataProvider>    ← context: STATE + tareas agrupadas
        ├── <Header>
        │   ├── <ModoMasterBadge>
        │   └── <RefreshButton>
        └── <Router>           ← hash-based, sin react-router
            ├── <Dashboard>            (#/)
            │   └── <AgentCard> × 4
            └── <AgentDetail>          (#/agent/:role)
                ├── <BackLink>
                ├── <AgentHeader>
                ├── <TaskList estado="pendiente">
                ├── <TaskList estado="en-curso">
                └── <TaskList estado="completado" limit={10}>
```

**Hooks custom:**
- `useRepoData()` → `{ state, tasks, refresh, lastPull, isLoading }`
- `useAgentData(role)` → derivado, filtra por rol
- `useRoute()` → parsea `window.location.hash`

**Estado global:** React Context, sin Redux/Zustand (ver ADR-001).

**Routing:** `location.hash`. Sin react-router para minimizar bundle.

---

## 3. Comandos Tauri

### Rust — `src-tauri/src/commands.rs`

```rust
use serde::Serialize;

#[derive(Serialize)]
pub struct Task {
    pub id: String,
    pub role: String,
    pub title: String,
    pub priority: String,
    pub estado: String,
    pub depende_de: Vec<String>,
    pub habilita: Vec<String>,
    pub file: String,
    pub log: Option<String>,
}

#[derive(Serialize)]
pub struct AgentSnapshot {
    pub role: String,
    pub ubicacion: String,
    pub ultima_senal: String,
}

#[derive(Serialize)]
pub struct StateSnapshot {
    pub updated: String,
    pub agents: Vec<AgentSnapshot>,
    pub modo_master_active: bool,
}

#[derive(Serialize)]
pub struct GitResult {
    pub ok: bool,
    pub message: String,
    pub head: String,
}

#[tauri::command]
pub fn list_tasks(estado: String, rol: Option<String>) -> Result<Vec<Task>, String>;

#[tauri::command]
pub fn read_state() -> Result<StateSnapshot, String>;

#[tauri::command]
pub fn read_task(archivo: String) -> Result<Task, String>;

#[tauri::command]
pub fn git_pull() -> Result<GitResult, String>;

#[tauri::command]
pub fn get_config() -> AppConfig;

#[tauri::command]
pub fn set_config(cfg: AppConfig) -> Result<(), String>;
```

### TypeScript — `src/lib/tauri.ts`

```ts
import { invoke } from "@tauri-apps/api/core";

export type Estado = "pendiente" | "en-curso" | "completado";
export type Role = "orchestrator" | "skills-curator" | "builder" | "auditor-ops";

export type Task = {
  id: string;
  role: Role;
  title: string;
  priority: "alta" | "media" | "baja";
  estado: Estado;
  dependeDe: string[];
  habilita: string[];
  file: string;
  log: string | null;
};

export type AgentSnapshot = {
  role: Role;
  ubicacion: string;
  ultimaSenal: string;
};

export type StateSnapshot = {
  updated: string;
  agents: AgentSnapshot[];
  modoMasterActive: boolean;
};

export type GitResult = { ok: boolean; message: string; head: string };

export const api = {
  listTasks: (estado: Estado, rol?: Role) =>
    invoke<Task[]>("list_tasks", { estado, rol }),
  readState: () => invoke<StateSnapshot>("read_state"),
  readTask: (archivo: string) => invoke<Task>("read_task", { archivo }),
  gitPull: () => invoke<GitResult>("git_pull"),
  getConfig: () => invoke<AppConfig>("get_config"),
  setConfig: (cfg: AppConfig) => invoke<void>("set_config", { cfg }),
};
```

### Tipos auxiliares — `src/types.ts`

```ts
export type AppConfig = {
  repoPath: string;          // ruta absoluta al clone del repo agente
  refreshTimeoutMs: number;  // default 5000
  autoRefreshOnFocus: boolean;
};
```

---

## 4. Config

**Ubicación:** `%APPDATA%/panel-agentes/config.json` (resuelto por `tauri::api::path::app_config_dir`).

**Default:**
```json
{
  "repoPath": "C:/Users/Tony/AppData/Local/Temp/agente-repo",
  "refreshTimeoutMs": 5000,
  "autoRefreshOnFocus": false
}
```

**Validación al arrancar:**
- `repoPath` no existe → modal "configurar ruta" antes de renderizar Dashboard.
- No es git repo → mismo modal, mensaje específico.

**Edición:** botón `⚙` en Header abre modal con los 3 campos.

---

## 5. Librerías externas mínimas

### Rust — `Cargo.toml`
- `tauri = "2"`, `tauri-build = "2"`
- `serde`, `serde_json`
- `pulldown-cmark = "0.10"` — parser markdown
- `chrono = "0.4"` — timestamps
- `git2 = "0.19"` — `git pull` sin shellear (no depende de `git.exe` en PATH)

### JS — `package.json`
- `react`, `react-dom`
- `@tauri-apps/api`, `@tauri-apps/plugin-dialog`
- `tailwindcss@^4`, `@tailwindcss/vite`
- `date-fns` — `"2026-04-19" → "hace 3h"`
- `clsx` — composición de clases Tailwind

**Excluidos:** react-router, Redux/Zustand, Material UI, date-fns-tz.

---

## 6. Parser de tareas (Rust)

Headings fijos:

| Sección | Marcador | Campo |
|---|---|---|
| Título | primer `# ` | `title` |
| Rol | `**Rol:** <rol>` | `role` |
| Prioridad | `**Prioridad:** alta\|media\|baja` | `priority` |
| Depende de | `## Depende de:` + bullets | `depende_de[]` |
| Habilita | `## Habilita:` + bullets | `habilita[]` |
| Log | `## Log del agente` → EOF | `log` |

Nombre archivo `builder-003-foo.md` → `role=builder`, `id=builder-003`.

---

## 7. Flujo de refresh

```
[click ↻] → api.gitPull()
             ↓
Rust: git2::Repository::open(repoPath).remote("origin").fetch
       + reset hard a origin/HEAD
             ↓ GitResult
<RepoDataProvider> re-fetch paralelo:
  ├─ readState()
  └─ listTasks("pendiente") + listTasks("en-curso") + listTasks("completado")
             ↓ setState
<Dashboard> y <AgentDetail> re-render.
```

Error en gitPull → toast rojo, data previa se conserva.

---

## 8. Sin backend HTTP

Confirmado. Evita: superficie de ataque server local, certs TLS self-signed, config firewall Windows.

---

## 9. Seguridad

- `tauri.conf.json` → `allowlist` con `fs:read` restringido a `repoPath`.
- Sin `shell.open`.
- Sin APIs remotas. Sin tokens en la app.

---

## Estado
aprobado

## Comentarios de revisión
(vacío)
