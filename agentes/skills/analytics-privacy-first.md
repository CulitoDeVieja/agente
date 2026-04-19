# analytics-privacy-first

## Propósito
Implementar analytics respetuosos de la privacidad del usuario con Plausible o Umami.

## Cuándo usar
- Apps que respetan la privacidad del usuario (sin cookies, sin GDPR banner)
- Métricas de uso básicas sin rastreo individual
- Alternativa a Google Analytics

## Pasos

### Opción A: Plausible (SaaS o self-hosted)

1. Agregar el script en `index.html`:
   ```html
   <script
     defer
     data-domain="miapp.com"
     src="https://plausible.io/js/script.js"
   ></script>
   ```

2. Eventos custom desde JS:
   ```ts
   // Plausible expone window.plausible
   declare function plausible(event: string, options?: { props?: Record<string, string> }): void;

   function trackEvent(name: string, props?: Record<string, string>) {
     if (typeof plausible !== 'undefined') {
       plausible(name, { props });
     }
   }

   trackEvent('Tarea creada', { priority: 'high' });
   trackEvent('Exportación', { format: 'PDF' });
   ```

### Opción B: Umami (self-hosted, open source)

1. Instalar Umami con Docker:
   ```yaml
   # docker-compose.yml
   services:
     umami:
       image: ghcr.io/umami-software/umami:postgresql-latest
       ports: ["3000:3000"]
       environment:
         DATABASE_URL: postgresql://umami:password@db:5432/umami
   ```

2. Agregar script (se obtiene desde el dashboard de Umami):
   ```html
   <script async defer
     src="https://analytics.miserver.com/script.js"
     data-website-id="uuid-de-tu-sitio"
   ></script>
   ```

3. Eventos custom con Umami:
   ```ts
   window.umami?.track('tarea-creada', { priority: 'high' });
   ```

### En Tauri (sin browser externo):
- Hacer llamadas HTTP desde Rust al endpoint de analytics
- O usar fetch desde el frontend si hay conexión

## Ejemplo
Plausible muestra: "1.2k usuarios activos esta semana, 68% usan la función de exportación."

## Errores comunes
- **Bloqueado por adblockers**: self-hostear bajo el propio dominio reduce el bloqueo.
- **Eventos sin contexto**: trackear solo eventos, no usuarios individuales (respetar el modelo privacy-first).
- **No funciona offline**: en apps Tauri offline, guardar eventos y enviarlos cuando hay conexión.
