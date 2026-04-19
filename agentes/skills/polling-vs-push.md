# polling-vs-push

## Propósito
Decidir entre polling (HTTP periódico), WebSockets o Server-Sent Events para datos en tiempo real.

## Cuándo usar
Consultar al diseñar sincronización de datos entre cliente y servidor.

## Pasos

### Comparación

| Criterio               | Polling                   | WebSockets               | SSE                      |
|------------------------|---------------------------|--------------------------|--------------------------|
| Complejidad cliente    | Baja                      | Media                    | Baja                     |
| Complejidad servidor   | Baja                      | Alta                     | Media                    |
| Bidireccional          | No                        | Sí                       | No (solo server→client)  |
| Latencia               | ~intervalo (alta)         | Muy baja                 | Baja                     |
| Overhead               | Alto (conexiones repetidas)| Bajo (conexión persistente)| Bajo                   |
| Funciona con HTTP/2    | Sí                        | No siempre               | Sí nativo                |
| Soporte Tauri          | Nativo (fetch)            | tokio-tungstenite        | eventsource en browser   |

### Elegir polling cuando:
- Actualizaciones cada 30+ segundos son suficientes
- El servidor no soporta conexiones persistentes
- Implementación rápida sin infraestructura extra
- Datos que no requieren baja latencia (sync de config, estado de job)

### Elegir WebSockets cuando:
- Necesidad de comunicación bidireccional (chat, colaboración)
- Latencia crítica (<1 s)
- Muchos clientes con actualizaciones frecuentes

### Elegir SSE cuando:
- Solo el servidor envía datos (feeds, notificaciones, logs en vivo)
- Se quiere simplicidad de SSE sobre la complejidad de WS
- El stack HTTP/2 ya está en uso

## Ejemplo

**Polling simple en React:**
```ts
useEffect(() => {
  const id = setInterval(() => refetch(), 30_000);
  return () => clearInterval(id);
}, []);
```

**SSE en React:**
```ts
useEffect(() => {
  const es = new EventSource('/api/events');
  es.onmessage = (e) => setData(JSON.parse(e.data));
  return () => es.close();
}, []);
```

## Errores comunes
- **Polling muy frecuente (<5 s)**: genera carga innecesaria; usar WS o SSE.
- **WebSockets sin reconexión**: implementar backoff exponencial para reconectar si cae la conexión.
- **SSE y reverse proxy**: nginx/caddy deben tener `proxy_buffering off` para SSE.
