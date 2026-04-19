# adr-template

## Propósito
Documentar decisiones de arquitectura (ADR) para que el equipo entienda el porqué de cada elección.

## Cuándo usar
Architect, cuando se toma una decisión técnica significativa que afecte a otros agentes o al proyecto a largo plazo.

## Pasos
1. Crear archivo `docs/adr/NNN-titulo-corto.md` en el repo del proyecto.
2. Completar la plantilla de abajo.
3. Commit: `adr: NNN titulo-corto`.

## Plantilla
```markdown
# NNN — Título corto

**Estado:** Aceptado | Propuesto | Deprecado

## Contexto
Qué situación o problema forzó esta decisión.

## Decisión
Qué se decidió hacer.

## Consecuencias
- **Positivas:** qué se gana.
- **Negativas / trade-offs:** qué se cede o complica.

## Alternativas descartadas
- Opción X → descartada porque Y.
```

## Errores comunes
- ADR sin contexto → futuro lector no entiende por qué se tomó la decisión.
- No documentar alternativas → parece que no hubo análisis.
- ADR para decisiones triviales → solo para decisiones que impactan arquitectura o son hard to reverse.
