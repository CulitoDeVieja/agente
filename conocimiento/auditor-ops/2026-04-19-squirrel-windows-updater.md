# Squirrel.Windows — DEPRECADO

## TL;DR
- Squirrel.Windows: **deprecado desde abril 2019**, no usar en proyectos nuevos.
- Sucesor moderno: **Velopack** (rewrite Rust, mismo CLI, migración auto).
- Para Tauri: el built-in `tauri-plugin-updater` es la opción nativa, NO Squirrel.

## Detalles

| Tool | Estado | Cuándo |
|---|---|---|
| Squirrel.Windows | ❌ Deprecado | Solo legacy maintenance |
| Clowd.Squirrel | ⚠️ Fork sucesor parcial | Migrar a Velopack |
| **Velopack** | ✅ Activo | App .NET / nativa que necesita installer + updater |
| **tauri-plugin-updater** | ✅ Activo | **Default para Tauri** |
| WinSparkle | ✅ Activo | C/C++ Windows desktop apps |
| MSIX + Microsoft Store | ✅ | Si querés distribución store-managed |

**Para panel-agentes (Tauri):** ignorar Squirrel/Velopack/WinSparkle. Usar `@tauri-apps/plugin-updater`:

```json
// tauri.conf.json
"plugins": {
  "updater": {
    "endpoints": ["https://github.com/CulitoDeVieja/panel-agentes/releases/latest/download/latest.json"],
    "pubkey": "<base64-public-key>"
  }
}
```

## Fuente
- https://github.com/Squirrel/Squirrel.Windows/issues/1469 (deprecation)
- https://velopack.io
- https://docs.velopack.io/migrating/squirrel

## Aplicabilidad
**No directa.** Tauri plugin-updater cubre nuestro caso. Velopack es backup si Tauri updater falla o si en el futuro hacemos app .NET. Documentar para no investigar de cero.
