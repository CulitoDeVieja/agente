# typescript-utility-types

## Propósito
Usar los tipos utilitarios built-in de TypeScript para transformar tipos existentes sin duplicar código.

## Cuándo usar
- Crear variantes de un tipo base (readonly, parcial, selección de campos)
- Extraer tipos de funciones o promesas existentes
- Evitar duplicación de interfaces similares

## Pasos

1. **Partial** — todos los campos opcionales:
   ```ts
   type Task = { id: string; title: string; done: boolean };
   type TaskUpdate = Partial<Task>; // { id?: string; title?: string; done?: boolean }
   ```

2. **Required** — todos los campos obligatorios:
   ```ts
   type StrictTask = Required<Task>;
   ```

3. **Pick** — seleccionar campos:
   ```ts
   type TaskPreview = Pick<Task, 'id' | 'title'>;
   ```

4. **Omit** — excluir campos:
   ```ts
   type NewTask = Omit<Task, 'id'>; // sin el id (lo asigna el servidor)
   ```

5. **Record** — diccionario tipado:
   ```ts
   type StatusMap = Record<string, Task[]>;
   const board: StatusMap = { todo: [], doing: [], done: [] };
   ```

6. **ReturnType** — extraer el tipo de retorno de una función:
   ```ts
   async function fetchUser() { return { id: '1', name: 'Ana' }; }
   type User = Awaited<ReturnType<typeof fetchUser>>;
   ```

7. **Parameters** — extraer parámetros:
   ```ts
   type FetchParams = Parameters<typeof fetch>; // [input: RequestInfo, init?: RequestInit]
   ```

8. **NonNullable** — eliminar null/undefined:
   ```ts
   type SafeId = NonNullable<string | null | undefined>; // string
   ```

9. **Readonly** — prevenir mutación:
   ```ts
   type ImmutableTask = Readonly<Task>;
   ```

## Ejemplo
```ts
function updateTask(id: string, patch: Omit<Partial<Task>, 'id'>) { ... }
```

## Errores comunes
- **`Partial` anidado no es deep**: `Partial<{ a: { b: string } }>` no hace `b` opcional.
- **`ReturnType` en async**: el tipo incluye `Promise<T>`; usar `Awaited<ReturnType<...>>`.
- **`Record` vs `Map`**: `Record` es un objeto JS normal; `Map` es una estructura de datos iterable.
