# Slot 0001 · feat · fetch-tareas-github-raw

**Fase:** 1 · Data layer
**Libre para tomar:** cualquier bot con skill React/TS/fetch
**Asignado:** (pendiente)
**Rama:** `slot-0001/fetch-tareas`

## Contexto

El panel-agentes necesita leer las tareas del repo `agente` SIN backend Rust (modo PWA web). La fuente son los archivos markdown en `tareas/pendiente/`, `tareas/en-curso/`, `tareas/completado/` del repo público `github.com/CulitoDeVieja/agente`.

## Objetivo

Crear un módulo TS que fetchee la lista de archivos de cada carpeta usando GitHub API contents.

## Archivo a crear

`panel-agentes/src/lib/fetchTareasGitHub.ts`

## Firma esperada

```ts
export type TareaFile = { name: string; path: string; estado: "pendiente" | "en-curso" | "completado" };

export async function fetchTareasList(
  owner = "CulitoDeVieja",
  repo = "agente"
): Promise<TareaFile[]>
```

## Lógica sugerida

```ts
async function listarCarpeta(estado: TareaFile["estado"]) {
  const url = `https://api.github.com/repos/${owner}/${repo}/contents/tareas/${estado}`;
  const r = await fetch(url);
  const files = await r.json();
  return files
    .filter((f: any) => f.name.endsWith(".md") && f.name !== ".gitkeep")
    .map((f: any) => ({ name: f.name, path: f.path, estado }));
}
// concat de los 3
```

## AC

- [ ] Archivo creado.
- [ ] `fetchTareasList()` devuelve array de todas las tareas con su estado.
- [ ] Manejo básico de error (si 404 o rate limit → lanza con mensaje claro).
- [ ] Test vitest `src/lib/fetchTareasGitHub.test.ts` con fetch mockeado (3 tests: éxito, vacío, error).

## Depende de: (ninguna)

## Habilita
- slot-0002 (parser md en TS)
- slot-0003 (hook useRepoData con fetch real)
