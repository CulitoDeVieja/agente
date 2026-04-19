# Renovate vs Dependabot

## TL;DR
- **Dependabot:** built-in GitHub, zero setup, suficiente para single-repo simple.
- **Renovate:** soporta 90+ ecosistemas, multi-platform, MUCHO mejor en monorepos.
- Ambos free. Si vivís 100% en GitHub y no es monorepo → Dependabot.

## Detalles
| Criterio | Dependabot | Renovate |
|---|---|---|
| Setup | `.github/dependabot.yml` | `renovate.json` + Mend app |
| Ecosistemas | 30+ | 90+ |
| Monorepo | Por package.json individual, no workspace-aware | Workspaces (npm/yarn/lerna), groups inteligentes |
| Plataformas | Solo GitHub | GitHub, GitLab, Bitbucket, Azure DevOps, Gitea |
| Auto-merge | Sí (con Action) | Sí (built-in, configurable por update type) |
| Schedule | Daily/weekly/monthly | Cron fino, "schedule": ["before 5am"] |

Para panel-agentes (single repo, GitHub-only, Tauri = Cargo + npm):
→ **Dependabot** alcanza. Si más adelante el sistema crece a monorepo (panel + DECK + MERCADO juntos): migrar a Renovate.

## Fuente
- https://docs.renovatebot.com/bot-comparison/
- https://appsecsanta.com/sca-tools/dependabot-vs-renovate
- https://dev.to/alex_aslam/renovate-vs-dependabot-which-bot-will-rule-your-monorepo-4431

## Aplicabilidad
**Sí — decisión arquitectural.** Default Dependabot para todos los repos del owner. Reservar Renovate si aparece monorepo real (no es el caso hoy: cada proyecto es repo separado).
