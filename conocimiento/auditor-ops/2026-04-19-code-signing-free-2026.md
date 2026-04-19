# Code signing Windows — opciones "free/cheap" 2026

## TL;DR
- **No hay certs realmente gratis** que disparen SmartScreen reputation.
- Azure Artifact Signing (ex Trusted Signing): ~$10/mes, GA, acepta self-employed (sin 3yr de business).
- SignPath = orquestador free para OSS; el cert sigue costando aparte.

## Detalles

| Opción | Costo | Cuándo |
|---|---|---|
| Azure Artifact Signing | ~$9.99/mes Azure sub paga | **Recomendado** para indie 2026; reemplaza EV cert |
| SignPath Foundation | Free para OSS (orquestación + cert donado por Certum) | Proyectos open source con releases públicas |
| Certum OV "Open Source" | ~$30/yr | OSS verificable, signal manual a Certum |
| Self-signed | $0 | Dev only — usuario ve "Publisher Unknown" |
| EV cert tradicional | ~$300-600/yr | Empresa con presupuesto, ya no es instant-trust desde 2024 |

**Para panel-agentes (indie, OSS):**
1. **v0.1.0 sin firma** → README explica cómo aprobar manualmente (paso `Más info → Ejecutar de todos modos`).
2. **v0.2.0+** → aplicar a SignPath Foundation (free para OSS).
3. **v1.0.0 con tracción real** → migrar a Azure Artifact Signing.

## Fuente
- https://azure.microsoft.com/en-us/products/artifact-signing
- https://learn.microsoft.com/en-us/azure/artifact-signing/faq
- https://melatonin.dev/blog/code-signing-on-windows-with-azure-trusted-signing/
- https://www.hanselman.com/blog/automatically-signing-a-windows-exe-with-azure-trusted-signing-dotnet-sign-and-github-actions

## Aplicabilidad
**Sí — decisión releases.** Reemplaza auditor-ops-90 (deploy-code-signing-cert). Ruta sugerida: SignPath Foundation primero, Azure Artifact Signing cuando el proyecto esté maduro.
