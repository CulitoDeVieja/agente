# 04 — Plan de auditoría + deploy

**Escribe:** auditor-ops
**Bloqueado hasta:** 03-implementacion aprobado.

## Auditoría

- [ ] Correr los tests del builder (mínimo 3). Verde obligatorio.
- [ ] Verificar que no hay strings "TODO" ni "FIXME" en `src/`.
- [ ] Buscar credenciales hardcodeadas (grep token, password, apiKey).
- [ ] Abrir la app localmente, verificar que:
  - Dashboard muestra 4 cards con números consistentes con el repo.
  - Click en card abre detalle.
  - Refrescar cambia la UI si el repo cambió.
- [ ] Escribir `notas/auditor/audit-panel-agentes-v0.1.md` con resultado.

## Deploy

- [ ] `cargo tauri build` → `.msi` y `.exe` portable.
- [ ] Tamaño final <15MB.
- [ ] Subir a `panel-agentes/releases/v0.1.0/`.
- [ ] Notificar al owner con ruta al release.

## Rollback
Si algo falla en producción (el owner reporta), revertir al tag anterior y reabrir issue al builder.

## Estado
borrador (esperando 03)

## Comentarios de revisión
(vacío)
