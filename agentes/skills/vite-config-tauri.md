# vite-config-tauri

## Propósito
Configurar Vite correctamente para desarrollo y build con Tauri 2.

## Cuándo usar
Al inicializar o ajustar la configuración de un proyecto Tauri + Vite.

## Pasos
`vite.config.ts` base para Tauri:
```ts
import { defineConfig } from "vite";
import react from "@vitejs/plugin-react";

export default defineConfig({
  plugins: [react()],
  clearScreen: false,
  server: { port: 1420, strictPort: true },
  envPrefix: ["VITE_", "TAURI_"],
  build: {
    target: "chrome105",
    minify: !process.env.TAURI_DEBUG,
    sourcemap: !!process.env.TAURI_DEBUG,
  },
});
```

## Ejemplo
```bash
# Dev con hot reload
cargo tauri dev   # lanza Vite en :1420 + Rust

# Build producción
cargo tauri build
```

## Errores comunes
- Puerto ocupado → `strictPort: true` falla; liberar :1420 o cambiar en `tauri.conf.json` también.
- Variables de entorno no disponibles → agregar prefijo `VITE_` o `TAURI_`.
- HMR no funciona → verificar `server.host` si se corre en VM/Docker.
- Build lento → activar `build.rollupOptions.output.manualChunks` para separar vendor.
