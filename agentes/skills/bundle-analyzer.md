# bundle-analyzer

## Propósito
Analizar el tamaño del bundle de producción con rollup-plugin-visualizer en proyectos Vite.

## Cuándo usar
- El bundle es más grande de lo esperado y se quiere identificar qué lo infla
- Antes de una optimización de performance
- Detectar dependencias duplicadas o no usadas

## Pasos

1. Instalar:
   ```bash
   npm install -D rollup-plugin-visualizer
   ```

2. Configurar en `vite.config.ts`:
   ```ts
   import { visualizer } from 'rollup-plugin-visualizer';

   export default defineConfig({
     plugins: [
       react(),
       visualizer({
         filename: 'dist/stats.html',
         open: true,           // abre automáticamente en el browser
         gzipSize: true,       // muestra tamaño gzipped
         brotliSize: true,
         template: 'treemap',  // opciones: treemap, sunburst, network
       }),
     ],
   });
   ```

3. Generar el análisis:
   ```bash
   npm run build
   # Se abre dist/stats.html automáticamente
   ```

4. Interpretar el treemap:
   - Bloques grandes = módulos pesados
   - Buscar: lodash, moment, lucide-react, date-fns completo
   - Detectar: misma librería importada dos veces con diferentes versiones

5. Optimizaciones comunes tras el análisis:
   ```ts
   // Importar solo lo necesario de una lib grande:
   import { format } from 'date-fns';          // bien
   import dateFns from 'date-fns';             // mal (incluye todo)

   // Code splitting manual:
   const HeavyComponent = lazy(() => import('./HeavyComponent'));

   // Excluir deps del bundle (externalizar):
   build: { rollupOptions: { external: ['electron'] } }
   ```

6. Alternativa: `vite-bundle-visualizer` (wrapper más simple):
   ```bash
   npx vite-bundle-visualizer
   ```

## Ejemplo
Análisis revela que `lucide-react` ocupa 800 KB; solución: importar íconos individuales.

## Errores comunes
- **Analizar en modo dev**: el bundle de dev no está optimizado; siempre analizar `npm run build`.
- **No usar gzipSize**: el tamaño raw engaña; el tamaño gzip es lo que recibe el usuario.
- **Ignorar chunks duplicados**: buscar el mismo módulo en múltiples chunks (tree-shaking roto).
