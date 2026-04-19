# Decisión: first-run-experience

**Decisión:** Primera abertura: modal "bienvenida + configurar ruta al repo". Si repo no existe, botón "clonar" que corre `git clone` a ruta default.

## Alternativas
- Asumir ruta default sin preguntar — falla si no está ahí.
- Full onboarding tour — overkill user único.
- Pantalla de login — sin auth.

## Justificación
Un solo paso: configurar ruta (o clonar). Después, app arranca normal. Config guardada, no vuelve a preguntar.
