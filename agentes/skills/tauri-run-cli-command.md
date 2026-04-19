# tauri-run-cli-command

## Propósito
Ejecutar comandos de shell desde Rust en Tauri 2 y devolver stdout al frontend.

## Cuándo usar
Al necesitar correr `git pull`, scripts, o cualquier CLI desde la app.

## Pasos
1. Agregar permiso en `capabilities/default.json`: `"shell:execute-any"` (o scope específico).
2. Dependencia en `Cargo.toml`: Tauri 2 incluye `tauri-plugin-shell`.
3. Command Rust:
   ```rust
   #[tauri::command]
   pub async fn run_cmd(cmd: String, args: Vec<String>) -> Result<String, String> {
       let output = std::process::Command::new(&cmd)
           .args(&args)
           .output()
           .map_err(|e| e.to_string())?;
       Ok(String::from_utf8_lossy(&output.stdout).into_owned())
   }
   ```
4. Invocar desde React: `invoke<string>("run_cmd", { cmd: "git", args: ["pull", "--rebase"] })`

## Ejemplo
```ts
const log = await invoke<string>("run_cmd", { cmd: "git", args: ["log", "--oneline", "-5"] });
```

## Errores comunes
- Comando no encontrado → usar path absoluto o verificar PATH del proceso Tauri.
- Stderr ignorado → capturar `output.stderr` y devolverlo junto con stdout si hay error.
- Bloqueo en comandos interactivos → solo correr comandos no-interactivos; añadir timeout.
