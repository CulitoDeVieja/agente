# Slot 0076 · feat · bugs-view-route

**Fase:** Fase3-Features
**Rama de habilidad:** react
**Libre para tomar:** bot con skill React Router + fetch
**Asignado:** (pendiente)
**Rama git:** `slot-0076/bugs-route`

## Qué hacer

Agregar ruta `/bugs` al router. Vista base que fetchea `notas/lupa/bugs.json` del repo `agente` y muestra placeholder "Vista de bugs — cargando…".

Los slots 0077-0085 agregan las features concretas (list, filter, search, detail modal, etc.).

## Pasos

1. Verificar si existe router; si no, instalar `react-router-dom`.
2. Crear `panel-agentes/src/pages/BugsView.tsx`.
3. Ruta `/bugs` en el router principal.
4. Link en header/sidebar para ir a `/bugs`.
5. Fetch `https://raw.githubusercontent.com/CulitoDeVieja/agente/master/notas/lupa/bugs.json` al montar.
6. Render: placeholder + count de bugs abiertos en header.

## API bugs.json

```ts
type BugsFile = {
  $schema: string;
  updated: string;
  curator: string;
  resumen: { total: number; cerrados: number; abiertos: number; ... };
  bugs: Bug[];
};
type Bug = {
  id: string;
  titulo: string;
  severidad: "alta" | "media" | "baja";
  estado: "abierto" | "cerrado";
  detectado_por?: string;
  fuente?: string;
  tarea_fix?: string | null;
  resuelto_en?: string;
  fecha_cierre?: string;
};
```

## AC

- [ ] Ruta `/bugs` activa.
- [ ] Fetch de bugs.json funciona + loading + error states.
- [ ] Link visible desde header.
- [ ] Test `BugsView.test.tsx` con 3 casos (loading, cargado, error).
- [ ] Types TS exportados para uso de slots 0077+.

## Depende de: (ninguna)

## Habilita
- slot-0077 (list), 0078 (filter severity), 0079 (filter estado), 0080..0085 (resto).
