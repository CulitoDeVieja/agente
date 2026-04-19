# vitest-coverage

## Propósito
Medir cobertura de código en proyectos Vite/React con Vitest y @vitest/coverage-v8.

## Cuándo usar
- Proyectos que ya usan Vitest para tests unitarios
- Necesidad de cobertura rápida integrada con el pipeline de Vite
- Alternativa moderna a Jest + Istanbul

## Pasos

1. Instalar:
   ```bash
   npm install -D @vitest/coverage-v8
   ```

2. Configurar en `vite.config.ts` o `vitest.config.ts`:
   ```ts
   import { defineConfig } from 'vitest/config';

   export default defineConfig({
     test: {
       coverage: {
         provider: 'v8',
         reporter: ['text', 'html', 'lcov'],
         include: ['src/**/*.{ts,tsx}'],
         exclude: [
           'src/**/*.spec.*',
           'src/**/*.test.*',
           'src/main.tsx',
           'src/vite-env.d.ts',
         ],
         thresholds: {
           lines: 80,
           branches: 70,
           functions: 80,
           statements: 80,
         },
         reportsDirectory: './coverage',
       },
     },
   });
   ```

3. Scripts en `package.json`:
   ```json
   {
     "scripts": {
       "test": "vitest",
       "test:run": "vitest run",
       "test:coverage": "vitest run --coverage"
     }
   }
   ```

4. Correr cobertura:
   ```bash
   npm run test:coverage
   # Abre el reporte HTML:
   open coverage/index.html
   ```

5. En GitHub Actions:
   ```yaml
   - run: npm run test:coverage
   - uses: codecov/codecov-action@v4
     with: { files: './coverage/lcov.info' }
   ```

6. Ver resumen en terminal:
   ```
   ----------|---------|----------|---------|---------|
   File      | % Stmts | % Branch | % Funcs | % Lines |
   ----------|---------|----------|---------|---------|
   tasks.ts  |   91.30 |    85.71 |  100.00 |   91.30 |
   ```

## Ejemplo
`vitest run --coverage --reporter=verbose` muestra tests + cobertura en una sola ejecución.

## Errores comunes
- **Provider `istanbul` vs `v8`**: v8 es más rápido pero menos preciso con ramas; istanbul es más exacto.
- **Archivos no incluidos**: verificar que los paths en `include` coincidan con la estructura real del proyecto.
- **Threshold falla en CI**: si es un proyecto nuevo, empezar sin `thresholds` y agregar gradualmente.
