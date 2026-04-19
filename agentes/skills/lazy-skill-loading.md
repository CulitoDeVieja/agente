# lazy-skill-loading

## Propósito
No cargar skills que no se usan → ahorro de tokens.

## Regla general
El agente carga solo:
1. Su manifest (`agentes/<rol>.md`).
2. Las skills base declaradas en su manifest.
3. Las skills que el **enunciado de la tarea** demanda.

## Cómo decidir qué cargar
1. Leer título + body del issue.
2. Identificar keywords (ej: "migración", "deploy", "react", "mercadopago").
3. Matchear contra `skills/INDEX.md`.
4. Cargar solo esas skills.

## Ejemplo
Issue: *"Builder: implementar endpoint `/payments/create-preference` con MercadoPago"*

El builder carga:
- Manifest propio
- Skills base (git, issues, worktree, test)
- **On-demand:** `skills/nodejs-express.md` + `skills/mercadopago-api.md`

No carga: `react-hooks.md`, `postgres-migrations.md`, `railway-deploy.md`, etc.

## Si falta una skill
1. Buscar en `INDEX.md` si existe pero con otro nombre.
2. Si no existe → abrir issue `skill-request` al Skills-Curator.
3. Mientras tanto, seguir con lo que tengas. NO inventar la skill en local.

## Métrica de éxito
Cada agente que use esta skill debería tener un prompt <50% del size de cargar todo el repo `agentes` completo.
