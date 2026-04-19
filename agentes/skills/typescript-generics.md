# typescript-generics

## Propósito
Escribir funciones, hooks y tipos reutilizables en TypeScript usando genéricos (generics).

## Cuándo usar
- Funciones que operan sobre distintos tipos manteniendo type safety
- Hooks de React que devuelven tipos inferidos del input
- Contenedores tipados (listas, wrappers, respuestas de API)

## Pasos

1. Función genérica básica:
   ```ts
   function first<T>(arr: T[]): T | undefined {
     return arr[0];
   }

   const n = first([1, 2, 3]);     // number | undefined
   const s = first(['a', 'b']);    // string | undefined
   ```

2. Genérico con constraint:
   ```ts
   function getProperty<T, K extends keyof T>(obj: T, key: K): T[K] {
     return obj[key];
   }

   const user = { name: 'Ana', age: 30 };
   getProperty(user, 'name');   // string
   getProperty(user, 'age');    // number
   // getProperty(user, 'email'); // Error: 'email' no existe en T
   ```

3. Tipo genérico para respuestas de API:
   ```ts
   type ApiResponse<T> = {
     data: T;
     error: string | null;
     loading: boolean;
   };

   type TaskResponse = ApiResponse<Task[]>;
   type UserResponse = ApiResponse<User>;
   ```

4. Hook genérico:
   ```ts
   function useLocalStorage<T>(key: string, defaultValue: T) {
     const [value, setValue] = useState<T>(() => {
       const stored = localStorage.getItem(key);
       return stored ? JSON.parse(stored) : defaultValue;
     });

     const set = (v: T) => {
       setValue(v);
       localStorage.setItem(key, JSON.stringify(v));
     };

     return [value, set] as const;
   }
   ```

5. Genérico con default:
   ```ts
   type Paginated<T = unknown> = { items: T[]; total: number; page: number };
   ```

## Ejemplo
`useLocalStorage<Settings>('settings', defaultSettings)` infiere el tipo automáticamente.

## Errores comunes
- **Genérico `any` por defecto**: `<T = any>` elimina el type safety; preferir `<T = unknown>`.
- **Constraint demasiado estricto**: no usar `object` como constraint si `Record<string, unknown>` es suficiente.
- **Inferencia rota con callbacks**: anotar el tipo del parámetro cuando TS no puede inferirlo.
