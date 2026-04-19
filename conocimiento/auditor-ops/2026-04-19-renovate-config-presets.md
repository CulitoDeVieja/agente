# Renovate — config presets

## TL;DR
- Presets son JSON/JSON5 reusables; un repo central con `default.json` + extends en cada proyecto.
- Preset oficial recomendado: `config:best-practices` (incluye recommended + pinDigests + maintainLockFiles).
- ⚠️ Presets npm-based deprecados; usar git-based.

## Detalles

**Config best-practices (incluye):**
- `config:recommended` (defaults sanos)
- `docker:pinDigests` (pin SHA en Docker tags)
- `helpers:pinGitHubActionDigests` (pin SHA en Actions — CRÍTICO post-Trivy 2026)
- `:configMigration`
- `:pinDevDependencies`
- `abandonments:recommended`
- `security:minimumReleaseAgeNpm`
- `:maintainLockFilesWeekly`

**Preset compartido propio (repo `culitodevieja/renovate-config`):**
```json
// default.json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": ["config:best-practices"],
  "timezone": "America/Argentina/Buenos_Aires",
  "schedule": ["before 5am on monday"],
  "prHourlyLimit": 2,
  "packageRules": [
    { "matchManagers": ["cargo"], "groupName": "rust deps" },
    { "matchManagers": ["npm"], "matchUpdateTypes": ["minor","patch"], "automerge": true }
  ]
}
```

En cada repo:
```json
// renovate.json
{ "extends": ["github>culitodevieja/renovate-config"] }
```

## Fuente
- https://docs.renovatebot.com/config-presets/
- https://docs.renovatebot.com/presets-config/
- https://github.com/renovatebot/renovate/issues/16100

## Aplicabilidad
**Sí — futuro.** Hoy default es Dependabot (ver renovate-vs-dependabot). Si el sistema multi-agente crece y migramos a Renovate, este preset evita config duplicada en panel-agentes + DECK + MERCADO.
