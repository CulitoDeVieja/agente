# Setup: CI workflow GitHub Actions en panel-agentes

**Rol:** auditor-ops (modo OPS)
**Severidad:** media
**Detectado por:** auditor-ops (subagente audit dinámico) — Lupa lo había mencionado, no se creó.

## Problema
Repo `CulitoDeVieja/panel-agentes` no tiene `.github/workflows/`. Sin CI:
- Bugs como LUPA-008 (camelCase) y LUPA-009 (build falla) no se detectan automáticamente.
- Cada commit del builder requiere audit manual del auditor.

## Fix (ops scope)
Crear `.github/workflows/ci.yml` con:
1. Job `frontend`: setup-node@v4 (20), npm ci, npm test, npm run build, npm audit.
2. Job `backend`: dtolnay/rust-toolchain@stable, cargo check, cargo clippy --all-targets -- -D warnings, cargo test (en `src-tauri/`).
3. Cargar `actions-rust-lang/audit@v2` para advisories Rust.
4. Trigger: PR + push a main.

## AC
- [ ] `.github/workflows/ci.yml` mergeado a main de panel-agentes.
- [ ] Primer run pasa al menos `frontend job` (después de fixes 004-010 deberían pasar todos).
- [ ] Status check visible en PRs futuros.

## Depende de: (ninguna técnica — pero ver bloqueo abajo)

## Bloqueo (2026-04-19, intento 1)
Push a panel-agentes rechazado:
```
! [remote rejected] main -> main (refusing to allow an OAuth App to create or update workflow `.github/workflows/ci.yml` without `workflow` scope)
```
`gh auth status` confirma: token actual tiene scopes `gist, read:org, repo` — falta `workflow`.

**Acción requerida del owner:** correr en su terminal local:
```bash
gh auth refresh -s workflow
```
Después puedo retomar la tarea sin más cambios. El `ci.yml` ya está escrito y testeado localmente, solo falta el push.

---
## Log auditor-ops (cierre por superación)
- Commit `520b70e` de Zeus/Lupa en panel-agentes ya creó `.github/workflows/ci.yml`.
- Mi push estaba bloqueado por scope `workflow` faltante; Zeus lo hizo desde su entorno.
- Tarea obsoleta — cerrada sin acción de mi parte.
- LUPA-015 cerrado.
