# Fix: import unused rompe build (`noUnusedLocals`)

**Rol:** builder
**Severidad:** ALTA
**Detectado por:** auditor-ops (subagente audit dinámico) → bug LUPA-009

## Problema
`src/components/AgentDetail.tsx:1` importa `Task` pero nunca lo usa.
Como `tsconfig.json` tiene `strict + noUnusedLocals` y `package.json:scripts.build = "tsc && vite build"`, el build de release **falla**. Bundle no se genera.

## Fix
```diff
- import { Task, AgentRole } from "@/types";
+ import type { AgentRole } from "@/types";
```

## AC
- [ ] `npm run build` exitoso, genera `dist/`.
- [ ] `npm test` sigue 7/7.

## Depende de: (ninguna)
