# semver-conventions

## Propósito
Aplicar versionado semántico (semver) consistente en proyectos y releases.

## Cuándo usar
Al crear tags, releases, o decidir qué número de versión usar.

## Reglas
- `MAJOR.MINOR.PATCH` → `1.2.3`
- **PATCH** (+0.0.1): bugfix sin cambios de API/UI.
- **MINOR** (+0.1.0): feature nueva backward-compatible.
- **MAJOR** (+1.0.0): breaking change o release inicial estable.
- Pre-release: `v0.x.x` = en desarrollo, API puede cambiar.

## Pasos
1. Primera release pública: `v0.1.0`
2. Bugfix: `v0.1.0` → `v0.1.1`
3. Feature nueva: `v0.1.1` → `v0.2.0`
4. Breaking change: `v0.2.0` → `v1.0.0`
5. Actualizar en `package.json` y `tauri.conf.json` antes de taggear.

## Ejemplo
```bash
# Después de bugfix
npm version patch   # 0.1.0 → 0.1.1
git tag v0.1.1
git push origin v0.1.1
```

## Errores comunes
- Saltarse versiones → confunde a usuarios; ir paso a paso.
- Olvidar actualizar `tauri.conf.json` → la app muestra versión vieja en "Acerca de".
- Tag sin `v` prefijo → convención estándar incluye `v`; ser consistente.
