# typescript-strict-mode

## Propósito
Activar TypeScript strict mode y resolver los errores comunes que genera.

## Cuándo usar
Al configurar un proyecto nuevo o migrar uno existente a tipado estricto.

## Pasos
1. En `tsconfig.json`: `"strict": true` (habilita nullChecks, noImplicitAny, etc.)
2. Resolver `possibly undefined`:
   ```ts
   // Antes: item.name (error si item puede ser undefined)
   // Después:
   item?.name ?? "sin nombre"
   ```
3. Tipar parámetros de funciones explícitamente.
4. Usar type guards para narrowing:
   ```ts
   if (typeof val === "string") { /* val es string aquí */ }
   ```

## Ejemplo
```ts
// tsconfig.json
{ "compilerOptions": { "strict": true, "noUncheckedIndexedAccess": true } }
```

## Errores comunes
- `Object is possibly undefined` → usar `?.` o guard explícito; no usar `!` (non-null assertion) salvo certeza absoluta.
- `any` implícito en callbacks → tipar el parámetro: `.map((item: Task) => ...)`
- Migración gradual → usar `// @ts-ignore` solo en archivos legacy marcados, nunca en código nuevo.
