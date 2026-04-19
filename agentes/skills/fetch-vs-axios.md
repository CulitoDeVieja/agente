# fetch-vs-axios

## Propósito
Decidir cuándo usar `fetch` nativo del browser y cuándo usar axios para peticiones HTTP.

## Cuándo usar
Consultar al iniciar un módulo de fetching o al revisar dependencias del proyecto.

## Pasos

### Comparación

| Criterio                  | fetch nativo               | axios                       |
|---------------------------|----------------------------|-----------------------------|
| Dependencia externa       | No                         | Sí (~13 KB)                 |
| JSON auto-parse           | No (`.json()` manual)      | Sí                          |
| Interceptores             | No (manual)                | Sí (nativo)                 |
| Timeout                   | No (AbortSignal manual)    | Sí (`timeout` option)       |
| Error en HTTP 4xx/5xx     | No lanza (check `ok`)      | Sí lanza automáticamente    |
| Node.js                   | Desde Node 18              | Sí (cualquier versión)      |
| Tauri / browser           | Nativo                     | Funciona                    |
| Cancelación               | AbortController            | CancelToken / AbortSignal   |

### Elegir fetch cuando:
- Proyecto con dependencias mínimas (app Tauri, biblioteca)
- Se usa TanStack Query (maneja el wrapper)
- Se quiere control total sobre la petición
- Solo se necesita JSON simple sin interceptores

### Elegir axios cuando:
- Se necesitan interceptores globales (auth headers, refresh token)
- El equipo prefiere la API más ergonómica
- Se maneja timeout declarativo
- Proyecto legacy que ya lo usa

## Ejemplo

**fetch con wrapper limpio:**
```ts
async function api<T>(url: string, init?: RequestInit): Promise<T> {
  const res = await fetch(url, { ...init, headers: { 'Content-Type': 'application/json' } });
  if (!res.ok) throw new Error(`HTTP ${res.status}`);
  return res.json();
}
```

**axios con interceptor de auth:**
```ts
const client = axios.create({ baseURL: '/api' });
client.interceptors.request.use(cfg => {
  cfg.headers.Authorization = `Bearer ${getToken()}`;
  return cfg;
});
```

## Errores comunes
- **fetch no lanza en 404**: siempre verificar `response.ok`.
- **axios en Tauri con CSP estricto**: preferir `fetch` para evitar problemas de cabeceras.
- **Instalar axios solo para `async/await`**: fetch nativo con async/await es suficiente para el 90% de los casos.
