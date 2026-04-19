# render-markdown-safely

## Propósito
Renderizar Markdown como HTML en React de forma segura, previniendo ataques XSS.

## Cuándo usar
- Notas, descripciones o comentarios escritos en Markdown por el usuario
- READMEs o documentación dentro de la app
- Cualquier contenido rich text que no sea HTML puro

## Pasos

1. Instalar:
   ```bash
   npm install react-markdown
   # Si se necesita sanitización extra:
   npm install rehype-sanitize remark-gfm
   ```

2. Uso básico con `react-markdown` (sanitiza por defecto):
   ```tsx
   import Markdown from 'react-markdown';

   function NoteContent({ markdown }: { markdown: string }) {
     return (
       <article className="prose dark:prose-invert prose-sm max-w-none">
         <Markdown>{markdown}</Markdown>
       </article>
     );
   }
   ```

3. Con soporte GFM (tablas, listas de tareas, strikethrough) y sanitización:
   ```tsx
   import Markdown from 'react-markdown';
   import remarkGfm from 'remark-gfm';
   import rehypeSanitize from 'rehype-sanitize';

   function SafeMarkdown({ content }: { content: string }) {
     return (
       <Markdown
         remarkPlugins={[remarkGfm]}
         rehypePlugins={[rehypeSanitize]}
       >
         {content}
       </Markdown>
     );
   }
   ```

4. Personalizar elementos renderizados:
   ```tsx
   <Markdown
     components={{
       a: ({ href, children }) => (
         <a href={href} target="_blank" rel="noopener noreferrer"
            className="text-brand-500 hover:underline">
           {children}
         </a>
       ),
       code: ({ children, className }) => (
         <code className={`${className} rounded bg-surface-alt px-1 py-0.5 font-mono text-sm`}>
           {children}
         </code>
       ),
     }}
   >
     {markdown}
   </Markdown>
   ```

5. NO usar `dangerouslySetInnerHTML` con Markdown sin sanitizar:
   ```tsx
   // PELIGROSO - no hacer:
   <div dangerouslySetInnerHTML={{ __html: marked(userContent) }} />

   // SEGURO:
   <Markdown rehypePlugins={[rehypeSanitize]}>{userContent}</Markdown>
   ```

## Ejemplo
Notas de usuario con `**negrita**`, `- listas` y `[links](url)` se renderizan sin riesgo de XSS.

## Errores comunes
- **`dangerouslySetInnerHTML` sin sanitizar**: cualquier `<script>` en el Markdown se ejecuta.
- **Links sin `rel="noopener"`**: los links externos deben tener `target="_blank" rel="noopener noreferrer"`.
- **Imágenes de dominios externos**: considerar bloquear `<img>` externas con sanitización custom si el contexto es sensible.
