# state-persistence

## Propósito
Persistir el estado de la UI entre sesiones usando localStorage, Tauri Store o archivos JSON.

## Cuándo usar
- Recordar preferencias del usuario (panel abierto, filtros activos, tema)
- Restaurar el estado de la app al reabrirla
- Guardar trabajo en progreso sin requerir guardado explícito

## Pasos

### Opción A: localStorage (web / Tauri frontend)

```ts
// Hook genérico de persistencia
function usePersistedState<T>(key: string, defaultValue: T) {
  const [state, setState] = useState<T>(() => {
    try {
      const stored = localStorage.getItem(key);
      return stored ? JSON.parse(stored) : defaultValue;
    } catch {
      return defaultValue;
    }
  });

  const setAndPersist = (value: T | ((prev: T) => T)) => {
    setState(prev => {
      const next = typeof value === 'function' ? (value as (p: T) => T)(prev) : value;
      localStorage.setItem(key, JSON.stringify(next));
      return next;
    });
  };

  return [state, setAndPersist] as const;
}

// Uso:
const [sidebarOpen, setSidebarOpen] = usePersistedState('sidebar-open', true);
```

### Opción B: tauri-plugin-store (más robusto, encriptable)

```bash
npm install @tauri-apps/plugin-store
```

```ts
import { load } from '@tauri-apps/plugin-store';

const store = await load('settings.json', { autoSave: true });

// Guardar
await store.set('theme', 'dark');
await store.set('lastProject', projectId);

// Leer
const theme = await store.get<string>('theme') ?? 'light';
```

### Opción C: Zustand + persist middleware

```ts
import { create } from 'zustand';
import { persist } from 'zustand/middleware';

const useUIStore = create(persist(
  (set) => ({
    sidebarOpen: true,
    activeFilter: 'all',
    toggleSidebar: () => set(s => ({ sidebarOpen: !s.sidebarOpen })),
  }),
  { name: 'ui-preferences' }
));
```

## Ejemplo
Al cerrar y reabrir la app, el sidebar está en el mismo estado y el filtro activo se restaura.

## Errores comunes
- **Datos sensibles en localStorage**: no guardar tokens ni datos privados; usar el plugin Store con encriptación.
- **JSON inválido en localStorage**: siempre envolver en try/catch al leer.
- **Estado desincronizado entre tabs**: localStorage sincroniza via evento `storage`; manejarlo si aplica.
