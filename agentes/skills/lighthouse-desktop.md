# lighthouse-desktop

## Propósito
Auditar performance, accesibilidad y best practices de apps desktop/web con Lighthouse CLI.

## Cuándo usar
- Medir baseline de performance antes de optimizar
- Detectar problemas de accesibilidad de forma automatizada
- Comparar métricas entre versiones del producto

## Pasos

1. Instalar Lighthouse CLI:
   ```bash
   npm install -g lighthouse
   # o como devDependency:
   npm install -D lighthouse
   ```

2. Auditar una URL:
   ```bash
   lighthouse http://localhost:5173 \
     --output html \
     --output-path ./reports/lighthouse.html \
     --view \
     --preset desktop
   ```

3. Auditar solo categorías específicas:
   ```bash
   lighthouse http://localhost:5173 \
     --only-categories performance,accessibility \
     --output json \
     --output-path ./reports/lighthouse.json
   ```

4. Integrar en scripts npm:
   ```json
   {
     "scripts": {
       "audit": "lighthouse http://localhost:4173 --preset desktop --output html --output-path dist/audit.html"
     }
   }
   ```

5. Leer resultados JSON en CI:
   ```ts
   import report from './reports/lighthouse.json';

   const perfScore = report.categories.performance.score * 100;
   const a11yScore = report.categories.accessibility.score * 100;

   if (perfScore < 80) process.exit(1);
   ```

6. Para Tauri: auditar el frontend cargado en la WebView:
   - Levantar el servidor de dev: `npm run dev`
   - Auditar la URL del dev server (normalmente `http://localhost:5173`)
   - La experiencia nativa (tray, notificaciones) no es evaluable por Lighthouse

## Ejemplo
```bash
lighthouse http://localhost:5173 --preset desktop --only-categories accessibility --output html --view
```

## Errores comunes
- **Lighthouse en modo mobile por defecto**: pasar `--preset desktop` para simular escritorio.
- **Resultados variables**: correr 3 veces y promediar; el primer run suele ser más lento.
- **Auditar en modo dev**: siempre auditar el build de producción (`npm run preview`) para métricas reales.
