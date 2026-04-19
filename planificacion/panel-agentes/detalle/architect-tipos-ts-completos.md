# Decisión: Tipos TypeScript completos

**Decisión:** Tipos centralizados en `src/types.ts` + `src/lib/tauri.ts` (tipos ligados a comandos). String literals para enums (sin `enum`).

## Alternativas
- **Enum nativo TS** — rechazado: runtime overhead + incompatibilidad con `erasableSyntaxOnly`.
- **Zod / io-ts para validación runtime** — rechazado v0.1: los datos vienen de Rust (serde), ya tipados en el wire. Revisar en v0.2 si se permite input de user.
- **Tipos por archivo (colocation)** — parcialmente: tipos de dominio en `types.ts`, tipos de comandos Tauri en `lib/tauri.ts`.

## Justificación
Ver `01-arquitectura.md §3`. String literal union (`"pendiente" | "en-curso" | "completado"`) es el idioma TS moderno: cero runtime, buen narrowing, serializable.
