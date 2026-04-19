# Trunk Check (meta-linter)

## TL;DR
- Meta-linter que orquesta 100+ linters/formatters con caching + daemon.
- "Hold-the-line": solo bloquea **issues nuevos** en PR, no exige fix de legacy.
- Más sofisticado que GitHub Super-Linter (output unificado, cache, LSP).

## Detalles

```bash
curl https://get.trunk.io -fsSL | bash
trunk init        # detecta lenguajes y arma .trunk/trunk.yaml
trunk check       # corre todos los linters habilitados
trunk fmt         # auto-format
```

**`.trunk/trunk.yaml` para Tauri (TS+Rust):**
```yaml
lint:
  enabled:
    - eslint@9.x
    - prettier@3.x
    - clippy@1.80.0
    - rustfmt@1.80.0
    - markdownlint@0.42.0
    - actionlint@1.7.x
    - gitleaks@8.24.2
    - trivy@0.55.x
```

Hold-the-line: si activás un nuevo linter en un repo viejo con 500 warnings, Trunk solo bloquea los warnings que aparecen en PRs nuevos. Migración incremental sin big-bang.

vs Super-Linter (GitHub): Super-Linter es un Docker image grande, no cachea entre runs. Trunk es CLI local + cloud cache, mucho más rápido.

## Fuente
- https://docs.trunk.io/check/supported-linters
- https://trunk.io/blog/trunk-check-one-linter-to-run-them-all
- https://github.com/trunk-io/trunk-action

## Aplicabilidad
**Parcial.** Trunk es excelente pero agrega una dependencia más. Para panel-agentes (proyecto chico): eslint+prettier+clippy directos alcanzan. Considerar Trunk si el sistema crece a múltiples repos con políticas compartidas.
