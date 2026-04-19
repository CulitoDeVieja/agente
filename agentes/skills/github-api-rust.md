# github-api-rust

## Propósito
Consumir la API REST de GitHub desde Rust usando `octocrab` o `reqwest` directamente.

## Cuándo usar
- Integrar datos de GitHub (repos, issues, releases) en una app Tauri
- Publicar releases o crear issues programáticamente
- Leer el CHANGELOG o releases desde la API para actualizaciones in-app

## Pasos

### Opción A: octocrab (cliente oficial de alto nivel)

1. Agregar en `Cargo.toml`:
   ```toml
   octocrab = "0.41"
   tokio = { version = "1", features = ["full"] }
   ```

2. Usar la API:
   ```rust
   use octocrab::Octocrab;

   async fn get_latest_release(owner: &str, repo: &str) -> Result<String, Box<dyn std::error::Error>> {
       let octocrab = Octocrab::builder()
           .personal_token(std::env::var("GITHUB_TOKEN")?)
           .build()?;

       let release = octocrab.repos(owner, repo)
           .releases()
           .get_latest()
           .await?;

       Ok(release.tag_name)
   }
   ```

### Opción B: reqwest directo (más control, sin dep extra)

1. Agregar en `Cargo.toml`:
   ```toml
   reqwest = { version = "0.12", features = ["json"] }
   serde = { version = "1", features = ["derive"] }
   ```

2. Llamar a la API REST:
   ```rust
   use serde::Deserialize;

   #[derive(Deserialize)]
   struct Release { tag_name: String, body: Option<String> }

   async fn fetch_latest_release(owner: &str, repo: &str) -> Result<Release, reqwest::Error> {
       let client = reqwest::Client::new();
       client
           .get(format!("https://api.github.com/repos/{owner}/{repo}/releases/latest"))
           .header("User-Agent", "mi-app/1.0")
           .header("Authorization", format!("Bearer {}", std::env::var("GITHUB_TOKEN").unwrap_or_default()))
           .send()
           .await?
           .json::<Release>()
           .await
   }
   ```

3. Exponer como comando Tauri:
   ```rust
   #[tauri::command]
   async fn check_for_updates() -> Result<String, String> {
       fetch_latest_release("mi-org", "mi-repo")
           .await
           .map(|r| r.tag_name)
           .map_err(|e| e.to_string())
   }
   ```

## Ejemplo
Al iniciar la app, consultar la última release y comparar con la versión actual para notificar actualizaciones.

## Errores comunes
- **Rate limit sin token**: la API pública limita a 60 req/hora; con token sube a 5.000.
- **`User-Agent` obligatorio**: sin este header, GitHub devuelve 403.
- **Token en el binario**: no hardcodear el token; leerlo de variables de entorno o del keychain del sistema.
