# Biome vs Prettier + ESLint — 2026

## TL;DR
- **Biome 2.3** (enero 2026): 10-25x más rápido que ESLint+Prettier, un solo binario Rust, 1 config file.
- Formateo drop-in compatible con Prettier. Type-aware linting via motor propio.
- **Limitación:** ~250 rules vs 1000+ de ESLint. Sin plugins framework-specific (`react-hooks`, `next`).

## Detalles

**Benchmarks (10k archivos):**
- Lint: Biome 0.8s vs ESLint 45.2s.
- Format: Biome 0.3s vs Prettier 12.1s.

**Formato:**
- Biome format ≈ Prettier output (diffs en edge cases).
- Una sola config `biome.json` reemplaza: `.eslintrc`, `.prettierrc`, `.eslintignore`, `.prettierignore`.

**Cuándo elegir Biome:**
- Proyecto nuevo JS/TS/React/Vue estándar.
- Speed importa (CI, pre-commit hooks).
- No necesitás plugins ESLint específicos (react-hooks, next, jsx-a11y custom).

**Cuándo quedarse con ESLint + Prettier:**
- Dependés de `eslint-plugin-react-hooks` o `eslint-plugin-next`.
- Necesitás rules específicas del ecosistema ESLint (accesibilidad, security).
- Migración completa es riesgosa en proyecto grande.

**Instalación:**
```bash
npm install --save-dev --save-exact @biomejs/biome
npx biome init
npx biome check --write ./src
```

## Fuente
- [Biome vs ESLint+Prettier 2026](https://www.pkgpulse.com/blog/biome-vs-eslint-prettier-linting-2026)
- [Biome: ESLint+Prettier Killer?](https://dev.to/pockit_tools/biome-the-eslint-and-prettier-killer-complete-migration-guide-for-2026-27m)

## Aplicabilidad a panel-agentes
**Sí — considerar para v0.2.** Panel-agentes ya tiene skill `eslint-prettier-setup.md`. Migración:
1. **Pro**: CI más rápido (panel chico, pero futuros proyectos crecen).
2. **Pro**: Un solo config simplifica onboarding nuevos agentes.
3. **Contra**: Perdemos `eslint-plugin-react-hooks` que detecta violaciones del React Compiler — **bloqueante** si activamos Compiler (ver nota evo 203).

**Decisión tentativa:** quedarse con ESLint + Prettier en v0.1 hasta que React Compiler esté activado. Reconsiderar para v0.2 si Biome suma el plugin react-hooks (en roadmap).

Alternativa a evaluar: **Oxlint** (también en Rust, más compatible con plugins ESLint que Biome).
