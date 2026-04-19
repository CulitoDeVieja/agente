# Plan: setup-tailwind

## Objetivo
Integrar Tailwind CSS en el proyecto.

## Inputs
- Vite + React + TS configurado (builder-011)

## Pasos
1. `npm install -D tailwindcss postcss autoprefixer`
2. `npx tailwindcss init -p`
3. Configurar `tailwind.config.js`: content apunta a `./src/**/*.{ts,tsx}`
4. Agregar directivas en `src/index.css`: `@tailwind base; @tailwind components; @tailwind utilities;`
5. Importar `index.css` en `src/main.tsx`

## Outputs
- `tailwind.config.js` configurado
- `postcss.config.js` generado
- Clases Tailwind disponibles en componentes

## Tests planeados
- [ ] Componente de prueba con clase `bg-blue-500` renderiza con color correcto
- [ ] `npm run build` no genera warnings de CSS
