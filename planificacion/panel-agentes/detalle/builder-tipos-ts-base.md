# Plan: tipos-ts-base

## Objetivo
Definir todos los tipos TypeScript base en `src/types.ts`.

## Tipos a definir
```ts
type AgentRole = "orchestrator" | "skills-curator" | "builder" | "auditor-ops";
type TaskEstado = "pendiente" | "en-curso" | "completado";
type TaskPriority = "alta" | "media" | "baja";

type Task = {
  id: string;
  role: AgentRole;
  title: string;
  priority: TaskPriority;
  estado: TaskEstado;
  dependeDe: string[];
  file: string;
  createdAt: string;
};

type AgentSnapshot = {
  role: AgentRole;
  ubicacion: string;
  ultimaSenal: string;
  pendientes: number;
  enCurso: number;
  completadas: number;
};

type StateSnapshot = {
  agents: AgentSnapshot[];
  modoMaster: boolean;
  lastUpdate: string;
};

type GitResult = { ok: boolean; message: string };
```

## Tests planeados
- [ ] Tipos importables sin errores desde componentes
- [ ] `tsc --noEmit` pasa
