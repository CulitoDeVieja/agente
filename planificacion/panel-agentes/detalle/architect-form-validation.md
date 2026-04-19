# Decisión: form-validation

**Decisión:** En config: validar `repoPath` existe + es git repo al submit. Mensaje inline bajo input. Sin libs.

## Alternativas
- Zod / React Hook Form — overkill para 3 campos.
- Validación al vuelo (blur) — molesta cuando usuario aún tipea.
- Solo validación server-side — no hay server.

## Justificación
3 inputs, validación on submit: `await invoke('validate_path')` → si falla, mostrar mensaje debajo del input.
