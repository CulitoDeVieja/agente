# release-please

## Propósito
Automatizar la generación de versiones y CHANGELOG con release-please de Google en GitHub Actions.

## Cuándo usar
- Proyectos que usan Conventional Commits y quieren releases automáticos
- Equipos que quieren PRs de release auto-generados en lugar de commits manuales
- Alternativa más simple a semantic-release para proyectos GitHub

## Pasos

1. Crear `.github/workflows/release-please.yml`:
   ```yaml
   name: Release Please

   on:
     push:
       branches: [main]

   permissions:
     contents: write
     pull-requests: write

   jobs:
     release-please:
       runs-on: ubuntu-latest
       steps:
         - uses: google-github-actions/release-please-action@v4
           with:
             token: ${{ secrets.GITHUB_TOKEN }}
             release-type: node
   ```

2. Configurar `release-please-config.json` en la raíz:
   ```json
   {
     "release-type": "node",
     "packages": {
       ".": {
         "release-type": "node",
         "bump-minor-pre-major": true,
         "changelog-sections": [
           { "type": "feat", "section": "Nuevas funciones" },
           { "type": "fix", "section": "Correcciones" },
           { "type": "perf", "section": "Mejoras de rendimiento" }
         ]
       }
     }
   }
   ```

3. Archivo `.release-please-manifest.json`:
   ```json
   { ".": "1.2.0" }
   ```

4. Flujo de trabajo:
   - Hacer commits con formato Conventional Commits (`feat:`, `fix:`, `chore:`)
   - release-please crea/actualiza un PR "Release v1.3.0"
   - Al mergear el PR, crea el tag y el GitHub Release automáticamente

5. Conventional Commits que generan bumps:
   - `fix:` → patch (1.0.1)
   - `feat:` → minor (1.1.0)
   - `feat!:` o `BREAKING CHANGE` en el footer → major (2.0.0)

## Ejemplo
PR automático: "chore(main): release 1.4.0" con CHANGELOG generado, listo para aprobar y mergear.

## Errores comunes
- **PR no se crea**: verificar que los permisos `pull-requests: write` estén en el workflow.
- **Versión no sube**: asegurarse de que los commits sigan el formato Conventional Commits exacto.
- **Multirepo**: usar `packages` en la config para monorepos con múltiples `package.json`.
