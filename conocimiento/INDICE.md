# Índice de conocimiento — base RAG privada por agente

Cada agente guarda acá hallazgos de investigación activa (loop evolución).

## Estructura

```
conocimiento/
├── orchestrator/       ← patrones coordinación, tooling multi-agente
├── skills-curator/     ← librerías nuevas, tech emergente, patrones skills
├── builder/            ← snippets, librerías descubiertas, hacks útiles
├── auditor-ops/        ← tools audit, CVEs, checklists, releases estándar
└── INDICE.md           ← curado por orchestrator (este archivo)
```

## Convención de archivo

```
<rol>/YYYY-MM-DD-<tema-slug>.md
```

Ejemplo: `builder/2026-04-19-tauri-v2-breaking-changes.md`.

## Regla antes de crear algo nuevo

**Grep primero** en `conocimiento/<tu-rol>/` antes de escribir una skill o componente. Si el conocimiento ya existe, reusá; no dupliques.

## Curado del orchestrator

Cada 7 días: consolidar este `INDICE.md` con links a los hallazgos más relevantes. Archivar lo obsoleto en `conocimiento/<rol>/archivo/`.
