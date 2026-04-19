# Snyk — free tier 2026

## TL;DR
- Free: 400 SCA + 100 SAST + 300 IaC + 100 Container tests/mes.
- **Repos públicos NO cuentan al límite + SCA ilimitado para open source.**
- Desde 01/2026 modelo nuevo de "Platform Credits" para licencias pagas.

## Detalles
Para panel-agentes (open source público):
- SCA (npm + Cargo) → ilimitado.
- Snyk Code (SAST) → 200 tests/mes para OSS.
- IaC, Container → no aplica (es desktop app).

GitHub Action:
```yaml
- uses: snyk/actions/node@master
  env: { SNYK_TOKEN: ${{ secrets.SNYK_TOKEN }} }
  with: { command: monitor }   # sube snapshot al dashboard
```

⚠️ Comparativa rápida:
- **Snyk Code (SAST):** UI buena, pocos falsos positivos, propietario.
- **Semgrep:** OSS, custom rules trivial.
- **CodeQL:** free GitHub, mejor detection en C++/Java, lento.

Para nuestro stack: Semgrep + Dependabot cubren 80% sin pagar token Snyk.

## Fuente
- https://snyk.io/plans/
- https://dev.to/rahulxsingh/snyk-pricing-in-2026-free-plan-team-business-and-enterprise-costs-breakdown-5e88
- https://appsecsanta.com/snyk

## Aplicabilidad
**Parcial.** Snyk Free es generoso para OSS pero agrega otro dashboard. Default: Semgrep + Dependabot + cargo-audit. Snyk solo si el owner quiere reportes consolidados visuales.
