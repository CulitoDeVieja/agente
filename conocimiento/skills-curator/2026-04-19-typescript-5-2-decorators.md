# TypeScript 5.2 — decorators + explicit resource management

## TL;DR
- TS 5.0 trae decorators Stage 3 (sin flag `experimentalDecorators`). TS 5.2 suma metadata de decorators.
- TS 5.2: `using` declarations — cleanup automático de recursos (`Symbol.dispose`).
- TS 5.2 quick wins: tuple elements named/unlabeled mezclados, methods on union array types, inline variable refactor.

## Detalles

**Decorators Stage 3 (TS 5.0):**
```ts
function logged(target: Function, context: ClassMethodDecoratorContext) {
  return function(this: any, ...args: any[]) {
    console.log(`Calling ${String(context.name)}`);
    return target.call(this, ...args);
  };
}

class Service {
  @logged
  fetch() { return fetch("/api"); }
}
```

- No usa `experimentalDecorators` (legacy Stage 2).
- Context parameter incluye `kind`, `name`, `addInitializer`, metadata.

**Decorator metadata (TS 5.2):**
```ts
function serialize(_target: any, context: ClassFieldDecoratorContext) {
  context.metadata[context.name] = true;
}

class Model {
  @serialize id!: string;
  @serialize title!: string;
}

const meta = Model[Symbol.metadata];  // { id: true, title: true }
```

**Explicit Resource Management (`using`):**
```ts
function getTempFile(): Disposable {
  return {
    path: "/tmp/x",
    [Symbol.dispose]() { /* cleanup */ }
  };
}

{
  using file = getTempFile();
  // file disposed automatically al salir del scope
}

// Async:
async function fetchStream() {
  await using conn = await openConnection();
  // conn[Symbol.asyncDispose]() al salir
}
```

**Tuple elements mixtos:**
```ts
type Pair<T> = [first: T, T];  // antes: error — ahora válido
```

**Methods on union arrays:**
```ts
declare let arr: string[] | number[];
arr.filter(x => !!x);  // antes error, ahora ok
```

**Auto-accessors (ya en TS 5.0):**
```ts
class X {
  accessor count = 0;  // equivale a get/set
}
```

## Fuente
- [TypeScript 5.2 Release Notes](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-5-2.html)
- [TypeScript 5.0 Decorators](https://devblogs.microsoft.com/typescript/announcing-typescript-5-0/)

## Aplicabilidad a panel-agentes
**Sí — usar `using` donde aplique.**

Casos concretos:
1. **File handles** en parse de tareas Markdown: `using f = openTaskFile(path)` → close automático si parser lanza error.
2. **DB connections** (si agregamos SQLite cache): `using db = openDb()` evita leaks.
3. **Metrics timers**: `using t = metricTimer("parse")` para medir tiempos sin manejar finally explícito.

Decorators: **no urgente.** Panel-agentes no usa classes con heavy patterns (validators, serializers). Código funcional con hooks es más idiomático en React. 

Skill existente `typescript-strict-mode.md` se mantiene. Agregar nota futura sobre `using` cuando escribamos la skill de file handling.

**Requisito:** `tsconfig.json` con `"target": "es2022"` o superior para que `Symbol.dispose` se polyfillee correctamente.
