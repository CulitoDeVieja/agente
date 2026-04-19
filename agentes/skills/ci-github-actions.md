# ci-github-actions

## Propósito
Configurar un pipeline de CI/CD con GitHub Actions para proyectos Tauri 2 + React.

## Cuándo usar
- Verificar que el build pasa en cada PR
- Correr tests automáticamente antes de mergear
- Publicar releases en múltiples plataformas

## Pasos

1. Crear `.github/workflows/ci.yml`:
   ```yaml
   name: CI

   on:
     push:
       branches: [main]
     pull_request:
       branches: [main]

   jobs:
     test-frontend:
       runs-on: ubuntu-latest
       steps:
         - uses: actions/checkout@v4
         - uses: actions/setup-node@v4
           with: { node-version: '20', cache: 'npm' }
         - run: npm ci
         - run: npm run lint
         - run: npm run test -- --run
         - run: npm run build

     test-rust:
       runs-on: ubuntu-latest
       steps:
         - uses: actions/checkout@v4
         - uses: dtolnay/rust-toolchain@stable
         - uses: Swatinem/rust-cache@v2
           with: { workspaces: './src-tauri -> target' }
         - run: cargo test --manifest-path src-tauri/Cargo.toml

     build-tauri:
       needs: [test-frontend, test-rust]
       strategy:
         matrix:
           os: [ubuntu-latest, windows-latest, macos-latest]
       runs-on: ${{ matrix.os }}
       steps:
         - uses: actions/checkout@v4
         - uses: actions/setup-node@v4
           with: { node-version: '20', cache: 'npm' }
         - run: npm ci
         - uses: dtolnay/rust-toolchain@stable
         - uses: Swatinem/rust-cache@v2
         - uses: tauri-apps/tauri-action@v0
           env:
             GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
   ```

2. Cachear dependencias para builds más rápidos:
   - `actions/setup-node` con `cache: 'npm'`
   - `Swatinem/rust-cache` para el target de Rust

3. Variables de entorno para signing (releases):
   ```yaml
   env:
     TAURI_SIGNING_PRIVATE_KEY: ${{ secrets.TAURI_KEY }}
     TAURI_SIGNING_PRIVATE_KEY_PASSWORD: ${{ secrets.TAURI_KEY_PASSWORD }}
   ```

## Ejemplo
Cada PR corre lint + tests en 2 min; build multiplataforma solo en merge a main.

## Errores comunes
- **Sin rust-cache**: cada build compila Tauri desde cero (~10 min); con cache baja a ~2 min.
- **Ubuntu sin deps de sistema**: Tauri necesita `libwebkit2gtk-4.1-dev`; la action de Tauri los instala.
- **Secrets no configurados**: fallar silenciosamente en signing; revisar Settings → Secrets.
