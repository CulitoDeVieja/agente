# Plan: scaffold-repo

## Objetivo
Crear repo `panel-agentes` en GitHub y estructura inicial Tauri 2 + React + TS.

## Inputs
- Token gh auth (del owner)
- Nombre org/user: CulitoDeVieja
- Nombre repo: panel-agentes

## Pasos
1. `gh repo create CulitoDeVieja/panel-agentes --private --clone`
2. `cd panel-agentes`
3. `npm create tauri-app@latest . -- --template react-ts` (preset React+TS)
4. Verificar que corre: `npm run tauri dev`
5. Primer commit: `feat: scaffold Tauri 2 + React + TS`
6. Push a main

## Outputs
- Repo `CulitoDeVieja/panel-agentes` creado en GitHub
- Estructura base:
  ```
  panel-agentes/
  ├── src-tauri/
  ├── src/
  ├── index.html
  ├── package.json
  └── vite.config.ts
  ```

## Tests planeados
- [ ] `npm run tauri dev` arranca sin errores
- [ ] `cargo check` en `src-tauri/` pasa
- [ ] Página default carga en webview

## Riesgos
- `gh auth` puede no estar disponible en VPS-2 → escalar al owner
- `cargo` / `rustup` pueden no estar instalados → verificar con `cargo --version`
