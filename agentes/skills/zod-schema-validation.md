# zod-schema-validation

## Propósito
Validar y parsear datos externos (APIs, formularios, archivos de config) con Zod en TypeScript.

## Cuándo usar
- Validar respuestas de API antes de usarlas en el estado
- Validar formularios en el cliente con mensajes de error tipados
- Parsear y tipar archivos de configuración JSON

## Pasos

1. Instalar:
   ```bash
   npm install zod
   ```

2. Definir un schema y derivar el tipo:
   ```ts
   import { z } from 'zod';

   const TaskSchema = z.object({
     id: z.string().uuid(),
     title: z.string().min(1, 'El título no puede estar vacío'),
     priority: z.enum(['low', 'medium', 'high']),
     dueDate: z.string().datetime().optional(),
     tags: z.array(z.string()).default([]),
   });

   type Task = z.infer<typeof TaskSchema>;
   ```

3. Parsear datos externos (lanza error si falla):
   ```ts
   const task = TaskSchema.parse(apiResponse); // Task tipada
   ```

4. Safe parse (no lanza, devuelve result):
   ```ts
   const result = TaskSchema.safeParse(apiResponse);
   if (!result.success) {
     console.error(result.error.flatten());
   } else {
     const task = result.data;
   }
   ```

5. Schemas para formularios con react-hook-form:
   ```ts
   import { zodResolver } from '@hookform/resolvers/zod';

   const FormSchema = z.object({
     title: z.string().min(1),
     email: z.string().email('Email inválido'),
   });

   const { register, handleSubmit, formState: { errors } } =
     useForm({ resolver: zodResolver(FormSchema) });
   ```

## Ejemplo
```ts
const Config = z.object({ theme: z.enum(['dark','light']), lang: z.string() });
const config = Config.parse(JSON.parse(fs.readFileSync('config.json', 'utf-8')));
```

## Errores comunes
- **`parse` vs `safeParse`**: usar `safeParse` en entradas no confiables para no propagar excepciones.
- **Tipos anidados**: `z.infer` solo infiere el nivel raíz; los tipos anidados también se infieren automáticamente.
- **`default` no valida**: `z.string().default('x')` no convierte; si el input es `null`, sigue fallando.
