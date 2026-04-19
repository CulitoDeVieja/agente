# tailwind-plugins

## Propósito
Usar plugins oficiales y crear plugins propios para extender Tailwind CSS con utilidades custom.

## Cuándo usar
- Agregar utilidades que Tailwind no incluye por defecto
- Encapsular patrones de diseño repetitivos en clases
- Integrar plugins de la comunidad (@tailwindcss/forms, etc.)

## Pasos

1. Instalar plugins oficiales:
   ```bash
   npm install @tailwindcss/forms @tailwindcss/typography
   ```

2. En Tailwind v4 (CSS-first), importar los plugins:
   ```css
   @import "tailwindcss";
   @plugin "@tailwindcss/forms";
   @plugin "@tailwindcss/typography";
   ```

3. Crear un plugin custom en v4:
   ```css
   /* Plugin inline en CSS */
   @layer utilities {
     .scrollbar-hidden {
       -ms-overflow-style: none;
       scrollbar-width: none;
     }
     .scrollbar-hidden::-webkit-scrollbar {
       display: none;
     }

     .text-balance {
       text-wrap: balance;
     }
   }
   ```

4. Plugin JS en `tailwind.config.js` (para v3 o v4 con config):
   ```js
   const plugin = require('tailwindcss/plugin');

   module.exports = {
     plugins: [
       plugin(({ addUtilities, addComponents, theme }) => {
         addUtilities({
           '.flex-center': {
             display: 'flex',
             alignItems: 'center',
             justifyContent: 'center',
           },
         });
         addComponents({
           '.card': {
             backgroundColor: theme('colors.white'),
             borderRadius: theme('borderRadius.xl'),
             padding: theme('spacing.6'),
             boxShadow: theme('boxShadow.md'),
           },
         });
       }),
     ],
   };
   ```

## Ejemplo
`className="card flex-center scrollbar-hidden"` combina los tres plugins del ejemplo.

## Errores comunes
- **Plugin v3 en proyecto v4**: la API de plugins cambió; preferir `@layer utilities` en v4.
- **`theme()` no disponible en CSS puro**: usar variables CSS `var(--spacing-4)` en lugar de `theme()`.
- **Clases no purgadas**: asegurar que los archivos donde se usan las clases estén en el `content` de Tailwind.
