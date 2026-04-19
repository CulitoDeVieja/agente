# Jotai vs Zustand — 2026

## TL;DR
- **Zustand**: single store, estado fuera del tree React. Simple, 3KB, el mejor default para estado global.
- **Jotai**: atoms primitivos componibles, estado dentro del tree React. 4KB, excelente para render-optimization automático.
- Uso común: TanStack Query (server) + Zustand (global client) + Jotai (form local).

## Detalles

**Filosofía:**

**Zustand:** un store central, acceso por hook con selector:
```ts
const useStore = create(set => ({
  tasks: [],
  setTasks: tasks => set({ tasks }),
}));
const tasks = useStore(s => s.tasks);
```

**Jotai:** atoms individuales, componibles:
```ts
const tasksAtom = atom([]);
const pendingAtom = atom(get => get(tasksAtom).filter(t => t.estado === "pendiente"));
const [tasks, setTasks] = useAtom(tasksAtom);
```

**Benchmarks (2026):**
- Bundle: Zustand 3KB, Jotai 4KB.
- Update 1000 subscribed components: Zustand 12ms, Jotai 14ms.
- Memory: Zustand 2.1MB, Jotai 1.8MB.

**Render optimization:**
- **Jotai**: automático — un atom cambia, solo componentes que leen ese atom re-renderizan.
- **Zustand**: manual — usar `useShallow(s => s.x)` para evitar re-renders extra.

**Cuándo usar cuál:**

| Caso | Ganador |
|---|---|
| Estado chico local (form, wizard) | Jotai |
| Global simple (user, theme) | Zustand |
| Necesitás DevTools Redux | Zustand |
| Code splitting es prioridad | Jotai |
| Actualizar state fuera React | Zustand |
| TypeScript first + Suspense | Jotai |

## Fuente
- [Zustand vs Jotai vs Redux Toolkit 2026 - Better Stack](https://betterstack.com/community/guides/scaling-nodejs/zustand-vs-redux-toolkit-vs-jotai/)
- [State Management 2026 - dev.to](https://dev.to/jsgurujobs/state-management-in-2026-zustand-vs-jotai-vs-redux-toolkit-vs-signals-2gge)
- [Jotai Comparison docs](https://jotai.org/docs/basics/comparison)

## Aplicabilidad a panel-agentes
**Zustand es el pick correcto.** Panel-agentes necesita:
1. Lista de tareas (global, compartida entre Dashboard y AgentDetail).
2. Filtros aplicados (compartidos entre sidebar y main view).
3. Refresh state (último pull, loading).

Todo esto encaja en **un solo store Zustand**. No hay form-state ni muchos atoms independientes que justifiquen Jotai.

Skill existente: `zustand-vs-context-decision.md` ya guía la decisión Zustand vs Context. **Sumar nota:** Jotai es alternativa razonable pero no necesaria para este proyecto.

**Regla práctica para el futuro:** si agregamos un form complejo multi-step (ej. wizard de setup inicial del agente), evaluar Jotai solo para ese form, no para todo el estado.
