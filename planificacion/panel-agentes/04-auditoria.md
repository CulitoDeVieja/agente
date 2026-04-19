# 04 — Plan de auditoría + deploy

**Escribe:** auditor-ops (psique)
**Referencia:** `03-implementacion.md` (builder) + `01-arquitectura.md` (architect) + `02-skills.md` (skills-curator)

---

## 1. Checklist de auditoría

### 1.1 Código
- [ ] **Tests pasan:** `cargo test` (Rust) + `pnpm test` (vitest JS). Verde obligatorio en las 2 suites.
- [ ] **Cobertura parser:** tests unitarios del parser markdown con >=5 fixtures distintas (pendiente/en-curso/completado, con y sin `## Depende de:`, con log).
- [ ] **Sin TODO/FIXME:** `rg -n "(TODO|FIXME|XXX|HACK)" src/ src-tauri/src/` vacío (salvo en comments que el owner apruebe explícitamente).
- [ ] **Sin `console.log`/`dbg!`:** `rg -n "console\.(log|debug|info)" src/` y `rg -n "dbg!" src-tauri/src/` vacío.
- [ ] **Sin `any` TS:** `rg -n ": any" src/` y `rg -n "as any" src/` vacío.
- [ ] **tsconfig estricto:** `"strict": true` + `"noUncheckedIndexedAccess": true`.
- [ ] **Clippy sin warnings:** `cargo clippy --all-targets -- -D warnings`.

### 1.2 Seguridad
- [ ] **Sin credenciales hardcodeadas:** `rg -in "(api[_-]?key|token|password|secret|bearer)" src/ src-tauri/src/` sin matches reales.
- [ ] **`tauri.conf.json` allowlist restringido:** `fs:read` solo sobre `repoPath`, sin wildcards.
- [ ] **Sin `shell.open` ni `shell.execute`:** confirmar ausentes en allowlist.
- [ ] **Sin remote APIs:** `rg -n "fetch\(" src/` vacío (no HTTP calls salvo por comandos Tauri).

### 1.3 UX (smoke test manual)
- [ ] App abre en <2s.
- [ ] Dashboard muestra 4 cards con números coherentes con `ls tareas/pendiente | grep <rol> | wc -l`.
- [ ] Click en card abre detalle; back link vuelve.
- [ ] Botón ↻ refresca; contadores actualizan si hay commit nuevo.
- [ ] Shortcuts (R, Esc, 1-4, `,`, `?`) funcionan.
- [ ] Config modal: cambia `repoPath` → app re-lee; inválido → modal bloqueante.

### 1.4 Métricas (`00-resumen.md`)
- [ ] `.msi` <15MB.
- [ ] App abre <2s.
- [ ] Refresh manual <1s.
- [ ] Cero credenciales hardcodeadas.

### 1.5 Entregable
- [ ] Reporte en `notas/auditor/audit-panel-agentes-v0.1.md` con cada checkbox y evidencia (output de comando o screenshot).

## 2. Plan de deploy

### 2.1 Build
1. `git pull --rebase` en `panel-agentes` branch `main`.
2. `pnpm install --frozen-lockfile`.
3. `pnpm build` (Vite + tsc).
4. `cargo tauri build --target x86_64-pc-windows-msvc`.
5. Output: `src-tauri/target/release/bundle/msi/panel-agentes_0.1.0_x64_en-US.msi` + `panel-agentes_0.1.0_x64-setup.exe`.

### 2.2 Smoke test post-build
- Instalar `.msi` en VM Windows limpia.
- Verificar checklist 1.3.
- Desinstalar.

### 2.3 Release
1. `git tag -a v0.1.0 -m "panel-agentes v0.1.0"` + `git push --tags`.
2. `gh release create v0.1.0 --title "v0.1.0" --notes-file release-notes-v0.1.0.md <artifacts>`.
3. Adjuntar: `.msi` + portable `.exe` + `release-notes`.
4. `release-notes-v0.1.0.md` incluye: features, requisitos, checksums SHA256.

### 2.4 Notificación al owner
- Log en tarea completada con URL del release, tamaño MSI, SHA256.

## 3. Plan de rollback

### Disparadores
- Owner reporta que app no abre.
- Crash en primer uso.
- Métrica fuera de rango (>20MB, >5s arranque, >3s refresh).
- Vulnerabilidad post-release.

### Pasos
1. `gh release delete v0.1.0 --yes` o marcar draft.
2. Reabrir tarea `builder-*` con detalle del fallo.
3. Actualizar `STATE.md` → modo master "bloqueado por incidente".
4. Crear `auditor-ops-*` post-mortem (timeline + causa raíz).
5. Re-release como `v0.1.1` post-fix.

### Backup
- Tag `v0.1.0-rc1` pre-release final, punto de retorno.
- No eliminar binarios locales por 72h de uso del owner.

---

## Estado
aprobado

## Comentarios de revisión
(vacío)
