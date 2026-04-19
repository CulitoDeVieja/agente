# github-releases-workflow

## Propósito
Crear releases en GitHub con assets adjuntos (binarios, instaladores) via CLI.

## Cuándo usar
Al publicar versión de una app o herramienta en GitHub.

## Pasos
1. Crear tag: `git tag v0.1.0 && git push origin v0.1.0`
2. Crear release: `gh release create v0.1.0 --title "v0.1.0" --notes "Primera release"`
3. Adjuntar assets: `gh release upload v0.1.0 ./build/panel-agentes.msi ./build/panel-agentes.exe`
4. Verificar: `gh release view v0.1.0`

## Ejemplo
```bash
gh release create v0.1.0 \
  --title "Panel Agentes v0.1.0" \
  --notes "App desktop para monitorear el sistema de agentes." \
  src-tauri/target/release/bundle/msi/*.msi \
  src-tauri/target/release/bundle/nsis/*.exe
```

## Errores comunes
- Tag no pusheado antes del release → `git push origin v0.1.0` primero.
- Asset demasiado grande → GitHub permite hasta 2GB por asset.
- Release draft por defecto → agregar `--draft=false` si se quiere publicar inmediatamente.
- Notes vacías → usar `--notes-file CHANGELOG.md` para notas largas.
