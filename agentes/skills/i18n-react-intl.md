# i18n-react-intl

## Propósito
Internacionalizar una app React con react-intl para soporte de múltiples idiomas y formatos.

## Cuándo usar
- App que necesita soporte para más de un idioma
- Formateo de fechas, números y monedas según locale
- Mensajes con plurales o variables interpoladas

## Pasos

1. Instalar:
   ```bash
   npm install react-intl
   ```

2. Crear archivos de traducción:
   ```json
   // src/i18n/es.json
   {
     "task.create": "Crear tarea",
     "task.count": "{count, plural, one {# tarea} other {# tareas}}",
     "task.due": "Vence el {date}"
   }
   ```

3. Configurar el provider:
   ```tsx
   // src/main.tsx
   import { IntlProvider } from 'react-intl';
   import es from './i18n/es.json';
   import en from './i18n/en.json';

   const messages = { es, en };
   const locale = navigator.language.startsWith('es') ? 'es' : 'en';

   ReactDOM.createRoot(document.getElementById('root')!).render(
     <IntlProvider locale={locale} messages={messages[locale]}>
       <App />
     </IntlProvider>
   );
   ```

4. Usar en componentes:
   ```tsx
   import { useIntl, FormattedMessage, FormattedDate } from 'react-intl';

   function TaskHeader({ count, dueDate }) {
     const intl = useIntl();

     return (
       <div>
         <FormattedMessage id="task.count" values={{ count }} />
         <FormattedDate value={dueDate} year="numeric" month="long" day="numeric" />
         <p>{intl.formatMessage({ id: 'task.create' })}</p>
       </div>
     );
   }
   ```

5. Formatear números y monedas:
   ```ts
   intl.formatNumber(1234.5, { style: 'currency', currency: 'EUR' });
   // → "1.234,50 €" (en locale es)
   ```

## Ejemplo
Cambiar `locale` de `es` a `en` re-renderiza toda la app con los textos en inglés.

## Errores comunes
- **ID de mensaje no encontrado**: react-intl muestra el ID en lugar del texto; verificar el JSON.
- **Plurales sin formato ICU**: usar la sintaxis `{count, plural, one {...} other {...}}` correctamente.
- **Fechas sin timezone**: especificar `timeZone` en `FormattedDate` para evitar desfases de un día.
