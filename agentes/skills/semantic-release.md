# semantic-release

## Propósito
Automatizar versiones, CHANGELOG y publicación de releases con semantic-release y Conventional Commits.

## Cuándo usar
- Proyectos que publican a npm o necesitan releases totalmente automáticos
- CI/CD donde no se quiere intervención manual en ningún paso del release
- Alternativa a release-please con más plugins y control

## Pasos

1. Instalar:
   ```bash
   npm install -D semantic-release \
     @semantic-release/git \
     @semantic-release/changelog \
     @semantic-release/github
   ```

2. Configurar `.releaserc.json`:
   ```json
   {
     "branches": ["main"],
     "plugins": [
       "@semantic-release/commit-analyzer",
       "@semantic-release/release-notes-generator",
       ["@semantic-release/changelog", {
         "changelogFile": "CHANGELOG.md"
       }],
       ["@semantic-release/git", {
         "assets": ["CHANGELOG.md", "package.json"],
         "message": "chore(release): ${nextRelease.version} [skip ci]"
       }],
       "@semantic-release/github"
     ]
   }
   ```

3. GitHub Actions workflow:
   ```yaml
   name: Release

   on:
     push:
       branches: [main]

   jobs:
     release:
       runs-on: ubuntu-latest
       steps:
         - uses: actions/checkout@v4
           with: { fetch-depth: 0 }
         - uses: actions/setup-node@v4
           with: { node-version: '20' }
         - run: npm ci
         - run: npx semantic-release
           env:
             GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
             NPM_TOKEN: ${{ secrets.NPM_TOKEN }}
   ```

4. Conventional Commits → versión:
   - `fix:` → patch
   - `feat:` → minor
   - `BREAKING CHANGE` en footer → major

## Ejemplo
Push a main con `feat: agregar exportación PDF` → semantic-release crea tag v2.1.0, actualiza CHANGELOG.md y publica el GitHub Release.

## Errores comunes
- **`fetch-depth: 0` obligatorio**: sin esto, semantic-release no puede leer el historial de git.
- **`[skip ci]` en el commit de release**: evitar loop infinito de CI.
- **NPM_TOKEN vacío sin publicar a npm**: si no se publica a npm, eliminar el token del env.
