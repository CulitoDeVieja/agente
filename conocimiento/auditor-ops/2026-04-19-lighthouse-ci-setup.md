# Lighthouse CI (LHCI) en GitHub Actions

## TL;DR
- LHCI 0.15.x corre Lighthouse 12.x en cada PR; falla build si rompe presupuesto.
- Action: `treosh/lighthouse-ci-action`.
- Budgets en `lighthouserc.json`: bundle size, FCP, LCP, score mínimo.

## Detalles
panel-agentes es Tauri (no web), pero el frontend Vite/React **se puede auditar** sirviéndolo standalone antes del bundle Tauri.

Workflow CI:
```yaml
- uses: actions/checkout@v4
- uses: actions/setup-node@v4
  with: { node-version: 20 }
- run: npm ci && npm run build && npm run preview &
- run: sleep 3
- uses: treosh/lighthouse-ci-action@v12
  with:
    urls: http://localhost:4173
    budgetPath: ./lighthouserc.json
    uploadArtifacts: true
```

`lighthouserc.json` ejemplo:
```json
{ "ci": { "assert": { "assertions": {
  "categories:performance": ["error", { "minScore": 0.9 }],
  "resource-summary:script:size": ["error", { "maxNumericValue": 300000 }]
}}}}
```

## Fuente
- https://github.com/GoogleChrome/lighthouse-ci
- https://github.com/treosh/lighthouse-ci-action
- https://googlechrome.github.io/lighthouse-ci/

## Aplicabilidad
**Sí condicional.** auditor-ops-031 mide Lighthouse manual; este workflow lo automatiza. Para Tauri: medir solo el frontend; Tauri agrega ~3MB de runtime que LHCI no mide. Para futuros proyectos web puros: aplicar full.
