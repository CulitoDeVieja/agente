# Fix: cambiar default branch a `main` en panel-agentes

**Rol:** auditor-ops
**Severidad:** alta
**Detectado por:** Lupa · test-report-v0.1.md
**Origen:** `auditor-ops-004-preparar-repo-panel-agentes` dejó `ops/init` como default.

## Qué fixear
Default branch del repo `github.com/CulitoDeVieja/panel-agentes` es `ops/init` (solo README). Debe ser `main` (donde está el código).

## Comando
```bash
gh api repos/CulitoDeVieja/panel-agentes -X PATCH -f default_branch=main
gh api repos/CulitoDeVieja/panel-agentes/git/refs/heads/ops/init -X DELETE
```

## AC
- [ ] `gh repo view CulitoDeVieja/panel-agentes --json defaultBranchRef` devuelve `main`.
- [ ] Clone fresh sin `-b` trae el código completo.
- [ ] Branch `ops/init` eliminada.

## Depende de: (ninguna — urgente)
