# Fix: README inconsistente con código real

**Rol:** builder
**Severidad:** media
**Detectado por:** auditor-ops (subagente audit dinámico) → bug LUPA-012

## Problema
README dice:
- `git2` (rechazado en ADR-001 — usamos `Command::new("git")`).
- `pnpm install --frozen-lockfile` (el repo trae `package-lock.json`, no `pnpm-lock.yaml`).

Cualquier nuevo colaborador que siga el README falla.

## Fix
- Cambiar `pnpm install --frozen-lockfile` → `npm ci`.
- Cambiar `pnpm run build` → `npm run build`.
- Sacar mención a `git2`; agregar referencia a ADR-001 explicando elección de `Command::new("git")`.

## AC
- [ ] README pasa "fresh clone test": clone + npm ci + npm test funciona del tirón.
- [ ] No menciona `git2` ni `pnpm`.

## Depende de: (ninguna)
