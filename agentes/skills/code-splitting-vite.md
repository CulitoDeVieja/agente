# code-splitting-vite

## Propósito
Dividir el bundle de Vite en chunks para mejorar tiempo de carga inicial.

## Cuándo usar
Si el bundle único supera ~500KB o hay rutas pesadas que no siempre se visitan.

## Pasos
1. Lazy load de rutas (ver `react-router-setup.md` y `react-suspense-basics.md`).
2. Separar vendor en `vite.config.ts`:
   ```ts
   build: {
     rollupOptions: {
       output: {
         manualChunks: {
           vendor: ["react", "react-dom"],
           router: ["react-router-dom"],
         }
       }
     }
   }
   ```
3. Verificar tamaños: `npx vite build --reporter=verbose`

## Ejemplo
```ts
// Chunk automático por ruta lazy
const AgentDetail = lazy(() => import("./pages/AgentDetail"));
// Vite genera agente-detail.[hash].js separado
```

## Errores comunes
- `manualChunks` con nombre duplicado → Rollup error; usar nombres únicos.
- Chunks demasiado pequeños → peor que un bundle grande por overhead de requests; chunks útiles son >20KB.
- En Tauri los assets son locales → el split igual ayuda por tiempo de parse JS, aunque no hay latencia de red.
