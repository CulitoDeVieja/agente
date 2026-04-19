# react-hooks-patterns

## Propósito
Patrones de hooks React para manejo de estado async, efectos y datos remotos.

## Cuándo usar
Al construir componentes que cargan datos o tienen estado complejo.

## Pasos
Hook para datos async:
```ts
function useAsyncData<T>(fetcher: () => Promise<T>) {
  const [data, setData] = useState<T | null>(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState<string | null>(null);

  useEffect(() => {
    setLoading(true);
    fetcher()
      .then(setData)
      .catch(e => setError(String(e)))
      .finally(() => setLoading(false));
  }, []);

  return { data, loading, error };
}
```

## Ejemplo
```tsx
const { data: tasks, loading } = useAsyncData(() =>
  invoke<Task[]>("list_tasks", { dir: "/opt/agente/tareas/pendiente" })
);
if (loading) return <Spinner />;
```

## Errores comunes
- Actualización de estado en componente desmontado → agregar cleanup con `AbortController` o flag `mounted`.
- `useEffect` con dependencias faltantes → activar eslint-plugin-react-hooks para detectar.
- Re-fetch innecesario → memoizar `fetcher` con `useCallback` si viene de props.
