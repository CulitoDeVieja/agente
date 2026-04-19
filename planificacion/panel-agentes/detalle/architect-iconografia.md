# Decisión: iconografia

**Decisión:** Lucide Icons (tree-shakeable). Iconos 16px en UI inline, 20px en cards, 24px en headers. Stroke width 2.

## Alternativas
- Heroicons — similares, tree-shake OK, pero Lucide tiene más variedad.
- Emoji — inconsistente across Windows versions.
- SVG custom inline — maintenance burden.

## Justificación
Lucide + tree-shake paga solo los iconos usados (~4KB por 10 icons). Estilo outline coherente con paleta oscura minimal.
