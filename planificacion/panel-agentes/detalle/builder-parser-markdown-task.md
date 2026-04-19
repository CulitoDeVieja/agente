# Plan: parser-markdown-task

## Objetivo
Implementar `src/lib/parseTask.ts` — parseo de archivos `.md` de tareas en TS.

## Función principal
```ts
function parseTask(content: string, filename: string): Task
function parseTaskList(files: RawFile[]): Task[]
```

## Pasos
1. Extraer título del `# H1`
2. Extraer campos `**Rol:**`, `**Prioridad:**`, `**Creada:**` con regex
3. Parsear sección `## Depende de:` — líneas que empiezan con `-`
4. Derivar `id` del filename sin extensión
5. Derivar `estado` del path (`pendiente`|`en-curso`|`completado`)

## Tests planeados (≥3 obligatorios)
- [ ] Parsea título correctamente de H1
- [ ] Extrae `dependeDe` como array vacío si `(ninguna)`
- [ ] Deriva `estado` del path del archivo
- [ ] Maneja archivo malformado sin crashear
