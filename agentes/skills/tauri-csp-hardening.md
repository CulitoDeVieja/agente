# tauri-csp-hardening

## Propósito
Configurar y endurecer el Content Security Policy (CSP) en Tauri 2 para prevenir XSS e inyección de código.

## Cuándo usar
- Antes de publicar una app Tauri en producción
- Al integrar contenido de terceros o iframes
- Cumplir requisitos de seguridad (App Store, auditorías)

## Pasos

1. Configurar CSP en `tauri.conf.json`:
   ```json
   {
     "app": {
       "security": {
         "csp": "default-src 'self'; script-src 'self'; style-src 'self' 'unsafe-inline'; img-src 'self' data: https:; connect-src 'self' https://api.miapp.com; font-src 'self' data:"
       }
     }
   }
   ```

2. Directivas CSP por tipo de recurso:

   | Directiva       | Uso recomendado                                 |
   |-----------------|-------------------------------------------------|
   | `default-src`   | `'self'` — fallback para todo                  |
   | `script-src`    | `'self'` — NO usar `'unsafe-eval'`             |
   | `style-src`     | `'self' 'unsafe-inline'` (necesario para CSS-in-JS) |
   | `img-src`       | `'self' data: https:` (para imágenes externas) |
   | `connect-src`   | Listar APIs permitidas explícitamente           |
   | `font-src`      | `'self' data:` (para fuentes embebidas)        |
   | `frame-src`     | `'none'` si no se usan iframes                 |

3. CSP para Tauri con nonces (más seguro que `unsafe-inline`):
   ```json
   {
     "csp": "default-src 'self'; script-src 'self' 'nonce-{TAURI_CSP_NONCE}'"
   }
   ```
   Tauri reemplaza `{TAURI_CSP_NONCE}` automáticamente.

4. Bloquear iframes completamente:
   ```json
   "csp": "... frame-src 'none'; frame-ancestors 'none';"
   ```

5. Deshabilitar características peligrosas:
   ```json
   {
     "app": {
       "security": {
         "dangerousDisableAssetCspModification": false,
         "freezePrototype": true
       }
     }
   }
   ```

6. Probar el CSP en desarrollo:
   - Abrir DevTools → Console → buscar errores de CSP
   - Los errores aparecen como: `Refused to load script from 'X' because it violates the following CSP directive`

## Ejemplo
CSP productivo mínimo: `"default-src 'self'; img-src 'self' data:; connect-src 'self' https://api.miapp.com"`

## Errores comunes
- **`unsafe-eval` en CSP**: permite evaluación de código JavaScript dinámico — nunca en producción.
- **Olvidar `connect-src` para la API**: las llamadas a la API fallarán silenciosamente sin esta directiva.
- **CSP bloqueando assets de Tauri**: Tauri modifica el CSP para incluir sus assets internos; no usar CSP completamente restrictivo.
