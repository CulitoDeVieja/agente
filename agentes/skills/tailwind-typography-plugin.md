# tailwind-typography-plugin

## Propósito
Estilizar contenido rich text (Markdown renderizado, artículos, documentación) con @tailwindcss/typography.

## Cuándo usar
- Renderizar Markdown o HTML externo con estilos tipográficos coherentes
- Documentación, posts de blog, notas de release
- Cualquier bloque de contenido con headings, párrafos, listas, código

## Pasos

1. Instalar:
   ```bash
   npm install @tailwindcss/typography
   ```

2. Activar en Tailwind v4:
   ```css
   @import "tailwindcss";
   @plugin "@tailwindcss/typography";
   ```

3. Usar la clase `prose` en el contenedor:
   ```tsx
   <article className="prose prose-lg max-w-none">
     <div dangerouslySetInnerHTML={{ __html: renderedMarkdown }} />
   </article>
   ```

4. Variantes de tamaño y color:
   ```tsx
   // Tamaños: prose-sm, prose (base), prose-lg, prose-xl, prose-2xl
   // Colores: prose-gray, prose-slate, prose-zinc, prose-neutral...

   <article className="prose prose-slate prose-lg dark:prose-invert">
     {content}
   </article>
   ```

5. Personalizar en `@theme`:
   ```css
   @theme {
     --typography-body: var(--color-text-primary);
     --typography-headings: var(--color-brand-500);
     --typography-code: var(--color-brand-300);
   }
   ```

6. Excluir elementos del estilo `prose`:
   ```tsx
   <article className="prose">
     <p>Texto estilizado</p>
     <div className="not-prose">
       <button className="btn-custom">Botón sin prose</button>
     </div>
   </article>
   ```

## Ejemplo
Vista de changelog con Markdown renderizado usando `prose dark:prose-invert prose-headings:text-brand-500`.

## Errores comunes
- **Estilos no aplican**: asegurarse de usar la clase en el contenedor padre del HTML, no en cada elemento.
- **Overflow en code blocks**: agregar `prose-pre:overflow-x-auto` si el código es muy ancho.
- **Dark mode**: usar `dark:prose-invert` para invertir colores automáticamente en modo oscuro.
