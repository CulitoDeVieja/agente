# Plan: setup-vite-react-ts

## Objetivo
Configurar Vite + React 18 + TypeScript en el repo panel-agentes.

## Inputs
- Repo `panel-agentes` ya scaffolded (builder-010)

## Pasos
1. Verificar `package.json` tiene `react`, `react-dom`, `typescript`, `vite`, `@vitejs/plugin-react`
2. Configurar `tsconfig.json`: strict mode, paths alias `@/` → `src/`
3. Configurar `vite.config.ts`: plugin react, resolve alias
4. Crear `src/main.tsx` con `ReactDOM.createRoot`
5. Crear `src/App.tsx` esqueleto

## Outputs
- `vite.config.ts` con alias configurado
- `tsconfig.json` con strict + paths
- `src/main.tsx` y `src/App.tsx` funcionales

## Tests planeados
- [ ] `npm run build` sin errores TypeScript
- [ ] `tsc --noEmit` pasa limpio
