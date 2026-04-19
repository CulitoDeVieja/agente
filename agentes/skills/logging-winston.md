# logging-winston

## Propósito
Implementar logging estructurado en Node.js con Winston, con salida a consola y archivos.

## Cuándo usar
- Backend Node.js que necesita logs en múltiples destinos (consola, archivo, servicio externo)
- Logs con niveles, timestamps y metadata estructurada
- Proyectos donde ya se usa Winston o el equipo lo conoce

## Pasos

1. Instalar:
   ```bash
   npm install winston
   ```

2. Crear el logger:
   ```ts
   // src/lib/logger.ts
   import winston from 'winston';

   const { combine, timestamp, json, colorize, simple } = winston.format;

   export const logger = winston.createLogger({
     level: process.env.LOG_LEVEL ?? 'info',
     format: combine(timestamp(), json()),
     transports: [
       new winston.transports.Console({
         format: process.env.NODE_ENV === 'production'
           ? combine(timestamp(), json())
           : combine(colorize(), simple()),
       }),
       new winston.transports.File({
         filename: 'logs/error.log',
         level: 'error',
         maxsize: 5 * 1024 * 1024, // 5 MB
         maxFiles: 3,
       }),
       new winston.transports.File({
         filename: 'logs/combined.log',
         maxsize: 10 * 1024 * 1024,
         maxFiles: 5,
       }),
     ],
   });
   ```

3. Usar el logger:
   ```ts
   logger.info('Servidor iniciado', { port: 3000 });
   logger.warn('Rate limit cerca', { userId, requests: 90 });
   logger.error('DB connection failed', { error: err.message, stack: err.stack });
   ```

4. Agregar metadata de contexto:
   ```ts
   const reqLogger = logger.child({ requestId: req.id, userId: req.user?.id });
   reqLogger.info('Request recibido', { method: req.method, path: req.path });
   ```

## Ejemplo
En producción, los logs van a `combined.log` como JSON; en desarrollo, coloridos en consola.

## Errores comunes
- **No rotar logs**: sin `maxsize`/`maxFiles`, el archivo crece indefinidamente.
- **Loguear objetos Error directamente**: Winston no serializa `Error` bien; usar `err.message` + `err.stack`.
- **Level incorrecto en producción**: setear `LOG_LEVEL=warn` en producción para reducir ruido.

### Winston vs Pino
- Winston: más flexible, múltiples transports, ecosistema maduro.
- Pino: 5-8x más rápido, ideal para alta carga. Ver `logging-pino.md`.
