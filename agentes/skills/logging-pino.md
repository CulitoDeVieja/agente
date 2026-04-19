# logging-pino

## Propósito
Implementar logging de alta performance en Node.js con Pino (5-8x más rápido que Winston).

## Cuándo usar
- APIs con alto throughput donde el logging es un cuello de botella
- Microservicios en producción que emiten logs a un agregador (Loki, Datadog)
- Proyectos nuevos Node.js sin deuda de Winston

## Pasos

1. Instalar:
   ```bash
   npm install pino pino-pretty
   ```

2. Crear el logger:
   ```ts
   // src/lib/logger.ts
   import pino from 'pino';

   export const logger = pino({
     level: process.env.LOG_LEVEL ?? 'info',
     transport: process.env.NODE_ENV !== 'production'
       ? { target: 'pino-pretty', options: { colorize: true } }
       : undefined,
     base: { service: 'mi-api', version: process.env.npm_package_version },
     timestamp: pino.stdTimeFunctions.isoTime,
   });
   ```

3. Usar el logger:
   ```ts
   logger.info('Servidor iniciado');
   logger.info({ port: 3000, env: 'production' }, 'Servidor iniciado');
   logger.warn({ userId }, 'Rate limit cerca');
   logger.error({ err }, 'Error procesando request'); // Pino serializa Error automáticamente
   ```

4. Child logger para contexto de request:
   ```ts
   const reqLog = logger.child({ requestId: req.id, userId: req.user?.id });
   reqLog.info({ path: req.path }, 'Request recibido');
   ```

5. Integración con Fastify (nativa):
   ```ts
   const app = Fastify({ logger: true }); // usa Pino internamente
   ```

6. Archivo de logs en producción:
   ```bash
   node server.js | pino-pretty > app.log  # dev
   node server.js >> /var/log/app.log       # producción (JSON puro)
   ```

## Ejemplo
```ts
logger.error({ err: new Error('DB timeout'), query }, 'Query fallida');
// Salida JSON: { "level": 50, "err": { "message": "DB timeout", "stack": "..." }, "query": "..." }
```

## Errores comunes
- **pino-pretty en producción**: solo para desarrollo; en producción emitir JSON puro.
- **`console.log` mezclado con Pino**: Pino escribe a stdout; `console.log` también; pueden interleavearse.
- **Nivel demasiado bajo en prod**: `debug` en producción puede generar GBs de logs/día.
