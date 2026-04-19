# tailwind-custom-theme

## Propósito
Extender el tema de Tailwind CSS v4 con colores, fuentes y variables custom usando CSS nativo.

## Cuándo usar
- Establecer la paleta de colores del producto (brand, neutral, semantic)
- Integrar fuentes personalizadas en las utilidades de Tailwind
- Crear tokens de diseño consistentes en toda la app

## Pasos

1. En Tailwind v4, el tema se define en CSS (no en `tailwind.config.js`):
   ```css
   /* src/index.css */
   @import "tailwindcss";

   @theme {
     --color-brand-500: #6d28d9;
     --color-brand-600: #5b21b6;
     --color-surface: #1e1e2e;
     --color-text-primary: #cdd6f4;
     --color-text-muted: #6c7086;

     --font-sans: 'Inter', ui-sans-serif, system-ui;
     --font-mono: 'JetBrains Mono', ui-monospace;

     --radius-card: 0.75rem;
     --spacing-content: 1.5rem;
   }
   ```

2. Usar los tokens en JSX:
   ```tsx
   <div className="bg-surface text-text-primary font-sans rounded-card p-content">
     Contenido
   </div>
   ```

3. Agregar fuente en HTML:
   ```html
   <link rel="preconnect" href="https://fonts.googleapis.com" />
   <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600&display=swap" rel="stylesheet" />
   ```

4. Variables semánticas para dark mode:
   ```css
   @theme {
     --color-bg: #ffffff;
   }

   .dark {
     --color-bg: #0f0f17;
   }
   ```

## Ejemplo
`className="bg-brand-500 hover:bg-brand-600 text-white"` usa los colores del theme custom.

## Errores comunes
- **v4 no usa `tailwind.config.js` para el theme**: la configuración es CSS-first; `extend` en config JS no aplica igual.
- **Nombre de variable inválido**: usar kebab-case; no usar `--colorBrand` sino `--color-brand-500`.
- **Fuente no se aplica**: verificar que `--font-sans` esté declarada dentro del bloque `@theme {}`.
