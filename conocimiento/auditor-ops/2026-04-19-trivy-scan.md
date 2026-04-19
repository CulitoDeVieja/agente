# Trivy (Aqua Security)

## TL;DR
- Scanner all-in-one: vulns + secrets + misconfigs + SBOM. OSS, free.
- Para repos: `trivy fs --scanners vuln,secret,misconfig .`
- ⚠️ **03/2026:** trivy-action sufrió supply chain compromise; pinear a SHA específico, NO usar tags.

## Detalles
Filesystem scan local:
```bash
trivy fs --scanners vuln,secret,misconfig --severity HIGH,CRITICAL .
```

GitHub Action (con SHA pin post-incidente):
```yaml
- uses: aquasecurity/trivy-action@<sha-validado>  # NO @master ni @v0.x
  with:
    scan-type: fs
    scanners: vuln,secret,misconfig
    severity: HIGH,CRITICAL
    exit-code: 1
```

Compara con OSV-Scanner: Trivy también detecta **secrets + misconfigs** (YAML, Dockerfile, Terraform). OSV solo vulns. Trivy gana en cobertura, OSV gana en velocidad.

## Fuente
- https://github.com/aquasecurity/trivy
- https://trivy.dev/
- https://thehackernews.com/2026/03/trivy-security-scanner-github-actions.html (incidente supply chain)

## Aplicabilidad
**Sí — con cuidado.** Para panel-agentes: trivy fs scan local + CI con SHA pin. Útil para detectar secrets accidentales y misconfig en GitHub Actions YAMLs. Documentar el incident del action en notas/auditor.
