# Decisión: Naming conventions

**Decisión:** React — `PascalCase` componentes, `camelCase` hooks/utils. Rust — `snake_case` funciones/módulos, `PascalCase` types/structs. Files — `PascalCase.tsx` componentes, `camelCase.ts` utils, `snake_case.rs` Rust.

## Alternativas
- **`kebab-case` para archivos React** — rechazado: convención TS/JSX es `PascalCase.tsx` para componentes; kebab lo usamos solo en markdown de tareas.
- **Hungarian notation (`strName`, `numCount`)** — rechazado: TS types ya expresan eso.
- **Abbreviations cortas (`agt`, `cfg`, `tsk`)** — rechazado: autocomplete es free; legibilidad > typing speed.

## Justificación
Standard de cada lenguaje. Evitar mezclar (ej: un `.tsx` no se llama `agent-card.tsx`). Exceptions: entradas `package.json`, scripts npm (kebab).

## Resumen
| Artefacto | Convención | Ejemplo |
|---|---|---|
| Componente React | `PascalCase.tsx` | `AgentCard.tsx` |
| Hook | `useXxx.ts` | `useRepoData.ts` |
| Util TS | `camelCase.ts` | `parseTask.ts` |
| Tipo TS | `PascalCase` | `AgentSnapshot` |
| Módulo Rust | `snake_case.rs` | `commands.rs` |
| Fn Rust | `snake_case` | `list_tasks` |
| Struct Rust | `PascalCase` | `Task` |
| Archivo tarea | `<rol>-NNN-kebab.md` | `architect-010-arbol-componentes.md` |
