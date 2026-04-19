# markdown-parsing

## Propósito
Parsear archivos markdown con YAML frontmatter y secciones desde Rust o TypeScript.

## Cuándo usar
Al leer tareas, skills o documentos del sistema de agentes que usan formato estándar.

## Pasos (Rust)
1. Dependencias en `Cargo.toml`: `serde = { features = ["derive"] }`, `serde_yaml`
2. Separar frontmatter: buscar `---` al inicio y segundo `---`
3. Parsear YAML entre los delimitadores con `serde_yaml::from_str`
4. El resto es body markdown — leer secciones por `## Título`

## Pasos (TypeScript)
1. `npm install gray-matter`
2. ```ts
   import matter from "gray-matter";
   const { data, content } = matter(rawText);
   ```

## Ejemplo (Rust)
```rust
fn parse_frontmatter(text: &str) -> Option<serde_yaml::Value> {
    let stripped = text.strip_prefix("---\n")?;
    let end = stripped.find("\n---")?;
    serde_yaml::from_str(&stripped[..end]).ok()
}
```

## Errores comunes
- Frontmatter sin newline final → `strip_prefix("---\n")` falla; normalizar con `trim_start`.
- Secciones con `###` confundidas con `##` → buscar exacto `\n## `.
- Encoding UTF-8 en Windows → leer con `read_to_string` que ya maneja UTF-8.
