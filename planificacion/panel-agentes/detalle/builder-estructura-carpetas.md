# Plan: estructura-carpetas

## Objetivo
Crear la estructura de carpetas del frontend React.

## Inputs
- Tailwind configurado (builder-013)

## Estructura a crear
```
src/
├── components/
│   ├── AgentCard.tsx
│   ├── AgentDetail.tsx
│   ├── TaskList.tsx
│   └── RefreshButton.tsx
├── lib/
│   ├── tauri.ts
│   └── parseTask.ts
├── types.ts
├── App.tsx
└── main.tsx
```

## Pasos
1. Crear carpetas `src/components/` y `src/lib/`
2. Crear archivos stub vacíos con export placeholder
3. Verificar imports no generan errores TS

## Tests planeados
- [ ] `tsc --noEmit` pasa con estructura vacía
