# Slot 0002 · feat · parse-task-markdown-ts

**Fase:** 1 · Data layer
**Libre para tomar:** cualquier bot con skill TS
**Asignado:** (pendiente)
**Rama:** `slot-0002/parse-task-md`

## Contexto

Necesitamos parsear el contenido markdown de las tareas (`.md` con YAML frontmatter tipo `**Rol:** builder`, y secciones `## Depende de:`, `## Habilita`, etc.) en TypeScript (para la versión PWA web sin Rust).

## Objetivo

Portar la lógica del parser Rust a TypeScript.

## Archivo a crear

`panel-agentes/src/lib/parseTaskMarkdown.ts`

## Firma esperada

```ts
export type TaskParsed = {
  id: string;
  role: string;
  title: string;
  priority: "alta" | "media" | "baja";
  estado: "pendiente" | "en-curso" | "completado";
  dependeDe: string[];
  habilita: string[];
  log: string | null;
  createdAt: string;
};

export function parseTaskMarkdown(
  content: string,
  filename: string,
  estado: TaskParsed["estado"]
): TaskParsed;
```

## Reglas de parsing

- `# Tarea: <título>` → `title` (trim "Tarea:" prefix).
- `**Rol:** X` → `role` (lowercase).
- `**Prioridad:** X` → `priority` (lowercase).
- `**Creada:** X` → `createdAt`.
- Sección `## Depende de:` → bullets o "(ninguna)" → `dependeDe` string[].
- Sección `## Habilita` → `habilita` string[].
- Sección `## Log del agente` → texto hasta siguiente `##` → `log`.
- `id` = filename sin `.md`.

## AC

- [ ] Archivo creado.
- [ ] Test `src/lib/parseTaskMarkdown.test.ts` con ≥5 casos: tarea completa, sin log, sin depende, sin habilita, tarea vacía.
- [ ] Cero `any`.

## Depende de: (ninguna — paralelo con slot-0001)

## Habilita
- slot-0003 (hook useRepoData que combina fetch + parse)

## Log del agente

**Asignado:** Claude PC (sesión 2026-04-22, en pausa de loops — orchestrator + builders pausados).
**Completado:** 2026-04-22.
**Commit:** `7a7ad4b` en `panel-agentes` main (merge de `slot-0002/parse-task-md`).

### Decisiones
- NO refactor del `src/services/taskParser.ts` existente (es la versión Tauri, deriva estado del path). El nuevo parser PWA es independiente. Decisión: minimizar superficie de cambio.
- Tests: 11 casos (más que el AC de 5) cubriendo edge cases de prioridad inválida, normalización de role lowercase, bullets con/sin `.md`.
- Cero `any` confirmado por `npx tsc --noEmit`.

### Tests
- 11/11 verdes en `src/lib/parseTaskMarkdown.test.ts`.
- Suite completa: 22/22 verdes (no rompió nada existente).

### Habilita ahora
- slot-0003 (hook `useRepoData` PWA combina fetch + este parser).
