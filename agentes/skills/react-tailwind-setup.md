# react-tailwind-setup

## Propósito
Configurar Tailwind CSS v4 en proyecto Vite + React + TypeScript.

## Cuándo usar
Al inicializar un proyecto frontend nuevo o agregar Tailwind a uno existente.

## Pasos
1. Instalar: `npm install tailwindcss @tailwindcss/vite`
2. En `vite.config.ts`:
   ```ts
   import tailwindcss from "@tailwindcss/vite";
   export default { plugins: [tailwindcss()] }
   ```
3. En `src/index.css`: `@import "tailwindcss";`
4. Importar en `main.tsx`: `import "./index.css"`
5. Verificar: agregar `className="text-red-500"` a cualquier elemento.

## Ejemplo
```tsx
export function Card({ title }: { title: string }) {
  return <div className="rounded-lg bg-gray-900 p-4 text-white">{title}</div>;
}
```

## Errores comunes
- Clases no aplican → verificar que `index.css` está importado en `main.tsx`.
- IntelliSense no sugiere clases → instalar extensión Tailwind CSS IntelliSense en VS Code.
- v3 vs v4 mezclados → v4 no usa `tailwind.config.js`; borrarlo si existe.
- Purge incompleto → en producción verificar que las clases dinámicas están en código estático.
