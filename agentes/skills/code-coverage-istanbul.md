# code-coverage-istanbul

## Propósito
Medir la cobertura de código en proyectos JavaScript/TypeScript con Istanbul (nyc).

## Cuándo usar
- Proyectos con Jest o Mocha que necesitan reportes de cobertura
- CI que requiere un umbral mínimo de cobertura
- Análisis de qué partes del código no están testeadas

## Pasos

1. Instalar:
   ```bash
   npm install -D nyc
   ```

2. Configurar en `package.json`:
   ```json
   {
     "nyc": {
       "extends": "@istanbuljs/nyc-config-typescript",
       "include": ["src/**/*.ts"],
       "exclude": ["src/**/*.spec.ts", "src/**/*.test.ts"],
       "reporter": ["text", "lcov", "html"],
       "branches": 70,
       "lines": 80,
       "functions": 80,
       "statements": 80,
       "check-coverage": true
     },
     "scripts": {
       "test:coverage": "nyc mocha 'tests/**/*.spec.ts'"
     }
   }
   ```

3. Para TypeScript, instalar:
   ```bash
   npm install -D @istanbuljs/nyc-config-typescript ts-node source-map-support
   ```

4. Con Jest (usa Istanbul internamente):
   ```json
   {
     "jest": {
       "collectCoverage": true,
       "coverageDirectory": "coverage",
       "coverageThreshold": {
         "global": { "branches": 70, "lines": 80 }
       },
       "coveragePathIgnorePatterns": ["/node_modules/", "/dist/"]
     }
   }
   ```

5. Ver el reporte HTML:
   ```bash
   npm run test:coverage
   open coverage/index.html
   ```

6. En GitHub Actions:
   ```yaml
   - run: npm run test:coverage
   - uses: codecov/codecov-action@v4
     with:
       files: coverage/lcov.info
   ```

## Ejemplo
`nyc report --reporter=text` muestra una tabla en consola con % por archivo.

## Errores comunes
- **Source maps incorrectos**: sin `source-map-support`, la cobertura apunta al JS transpilado, no al TS.
- **Umbral muy alto desde el inicio**: empezar en 60-70% y subir gradualmente.
- **No ignorar archivos de test**: la cobertura de los propios tests distorsiona el reporte.
