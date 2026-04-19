# changelog-generation

## Propósito
Generar y mantener un archivo CHANGELOG.md automáticamente a partir del historial de commits.

## Cuándo usar
- Documentar cambios entre versiones para usuarios y colaboradores
- Parte de un pipeline de release automatizado
- Proyectos open source que necesitan notas de release claras

## Pasos

### Opción A: git-cliff (recomendado, Rust-based, muy configurable)

1. Instalar:
   ```bash
   cargo install git-cliff
   # o con npm:
   npm install -D git-cliff
   ```

2. Generar el CHANGELOG:
   ```bash
   git cliff --output CHANGELOG.md        # desde el inicio
   git cliff --latest --output CHANGELOG.md  # solo el último tag
   git cliff v1.0.0..HEAD                 # rango específico
   ```

3. Configurar `cliff.toml`:
   ```toml
   [changelog]
   header = "# Changelog\n\n"
   body = """
   ## [{{ version }}] - {{ timestamp | date(format="%Y-%m-%d") }}
   {% for group, commits in commits | group_by(attribute="group") %}
   ### {{ group }}
   {% for commit in commits %}
   - {{ commit.message }}{% endfor %}
   {% endfor %}
   """

   [git]
   conventional_commits = true
   commit_parsers = [
     { message = "^feat", group = "Nuevas funciones" },
     { message = "^fix", group = "Correcciones" },
     { message = "^perf", group = "Rendimiento" },
     { message = "^chore\\(release\\)", skip = true },
   ]
   ```

### Opción B: conventional-changelog-cli

1. Instalar:
   ```bash
   npm install -D conventional-changelog-cli
   ```

2. Generar:
   ```bash
   npx conventional-changelog -p angular -i CHANGELOG.md -s
   ```

### En CI (GitHub Actions):
```yaml
- run: git cliff --latest --output CHANGELOG.md
- run: git add CHANGELOG.md && git commit -m "docs: update changelog [skip ci]"
```

## Ejemplo
`git cliff --bump --output CHANGELOG.md` — calcula la próxima versión automáticamente.

## Errores comunes
- **Commits sin formato convencional**: el 80% del valor depende de usar `feat:`, `fix:`, etc.
- **CHANGELOG duplicado**: usar `--latest` o `--unreleased` para no repetir entradas.
- **Commit de changelog en CI**: agregar `[skip ci]` al mensaje para no triggear otro pipeline.
