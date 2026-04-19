# eslint-prettier-setup

## Propósito
Configurar ESLint + Prettier en proyecto Vite + React + TypeScript.

## Cuándo usar
Al inicializar un proyecto o estandarizar estilo de código.

## Pasos
1. Instalar: `npm install -D eslint prettier eslint-config-prettier @typescript-eslint/eslint-plugin`
2. `eslint.config.js`:
   ```js
   import tseslint from "@typescript-eslint/eslint-plugin";
   import prettier from "eslint-config-prettier";
   export default [tseslint.configs.recommended, prettier];
   ```
3. `.prettierrc`:
   ```json
   { "semi": true, "singleQuote": false, "printWidth": 100 }
   ```
4. Scripts en `package.json`:
   ```json
   "lint": "eslint src", "format": "prettier --write src"
   ```

## Ejemplo
```bash
npm run lint    # detecta errores
npm run format  # formatea automáticamente
```

## Errores comunes
- ESLint y Prettier en conflicto → `eslint-config-prettier` desactiva reglas que chocan; debe ir al final del array.
- Reglas muy estrictas en inicio → usar `warn` en lugar de `error` para reglas nuevas.
- No formatea al guardar en VS Code → agregar `"editor.formatOnSave": true` y seleccionar Prettier como formatter.
