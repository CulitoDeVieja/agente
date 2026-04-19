# date-fns-usage

## Propósito
Formatear y manipular fechas con date-fns v3 en TypeScript.

## Cuándo usar
Al mostrar fechas de tareas, logs o timestamps en la UI.

## Pasos
1. Instalar: `npm install date-fns`
2. Importar solo las funciones necesarias (tree-shakeable):
   ```ts
   import { format, formatDistanceToNow, parseISO } from "date-fns";
   import { es } from "date-fns/locale";
   ```
3. Formatear: `format(new Date(dateStr), "dd/MM/yyyy")`
4. Relativo: `formatDistanceToNow(parseISO(dateStr), { addSuffix: true, locale: es })`

## Ejemplo
```ts
const creada = "2026-04-19";
format(parseISO(creada), "d 'de' MMMM yyyy", { locale: es }); // "19 de abril 2026"
formatDistanceToNow(parseISO(creada), { addSuffix: true, locale: es }); // "hace 2 días"
```

## Errores comunes
- `Invalid Date` → verificar formato de entrada; usar `parseISO` para strings ISO.
- Bundle grande → importar funciones individualmente, no `import * as dateFns`.
- Timezone incorrecto → date-fns opera en timezone local; usar `date-fns-tz` si se necesita UTC explícito.
