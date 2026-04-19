# zustand-vs-context-decision

## Propósito
Decidir entre Zustand y React Context para manejo de estado global.

## Cuándo usar
Al elegir solución de estado compartido en una app React.

## Regla de decisión
- **React Context**: estado simple, cambia poco, ≤2 consumers. Ej: tema, idioma, usuario auth.
- **Zustand**: estado que cambia frecuente, múltiples consumers, lógica de actualización compleja.

## Zustand setup (si se elige)
1. `npm install zustand`
2. ```ts
   import { create } from "zustand";
   interface AppStore {
     agents: Agent[];
     setAgents: (agents: Agent[]) => void;
   }
   export const useAppStore = create<AppStore>(set => ({
     agents: [],
     setAgents: agents => set({ agents }),
   }));
   ```
3. Usar: `const agents = useAppStore(s => s.agents)`

## Ejemplo
```ts
// Context → OK para tema
const ThemeContext = createContext<"dark" | "light">("dark");

// Zustand → OK para lista de tareas que se refresca cada 60s
const tasks = useAppStore(s => s.tasks);
```

## Errores comunes
- Context para estado frecuente → re-renders innecesarios en toda la árbol.
- Zustand para 1 componente → overkill; usar `useState` local.
- Selector faltante en Zustand → `useAppStore()` sin selector suscribe a todo el store.
