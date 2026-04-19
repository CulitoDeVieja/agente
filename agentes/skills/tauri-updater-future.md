# tauri-updater-future

## Propósito
Referencia para implementar auto-updater en Tauri 2 en versiones futuras.

## Cuándo usar
Cuando el proyecto madure y se quiera distribuir actualizaciones automáticas.

## Pasos (esquema)
1. Activar plugin: `npm install @tauri-apps/plugin-updater`
2. En `tauri.conf.json`:
   ```json
   "plugins": { "updater": { "active": true, "endpoints": ["https://releases.myapp.com/{{target}}/{{current_version}}"] } }
   ```
3. Firmar builds: Tauri updater requiere firma criptográfica.
4. Servidor de actualizaciones: GitHub Releases sirve como endpoint con formato correcto.
5. Check en app: `import { check } from "@tauri-apps/plugin-updater"`

## Nota
Esta skill es referencia futura. Para v0.1.0 del panel-agentes, no implementar todavía — complejidad no justificada en fase inicial.

## Errores comunes
- Olvidar firmar los binarios → updater rechaza instalación.
- Endpoint incorrecto → app no encuentra actualizaciones silenciosamente.
- Sin manejo de error en check → si el servidor está caído, la app no debe crashear.
