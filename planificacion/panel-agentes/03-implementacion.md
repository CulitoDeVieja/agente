# 03 — Plan de implementación (SIN CÓDIGO)

**Escribe:** builder
**Bloqueado hasta:** 01-arquitectura + 02-skills aprobados.

## Pasos que voy a dar (orden)

1. [ ] Crear repo `panel-agentes` (requiere gh auth — pedir al owner).
2. [ ] Scaffold Tauri: `npm create tauri-app@latest` con preset React+TS.
3. [ ] Configurar Tailwind.
4. [ ] Implementar comandos Rust (`list_tasks`, `read_state`, `read_task`, `git_pull`).
5. [ ] Implementar tipos TS en `src/types.ts`.
6. [ ] Implementar parser markdown en `src/lib/parseTask.ts` + test unitario.
7. [ ] Implementar `AgentCard.tsx` + `Dashboard` (ruta `/`).
8. [ ] Implementar `AgentDetail.tsx` (ruta `/agent/:role`).
9. [ ] Implementar `RefreshButton` con feedback visual.
10. [ ] Tests: al menos 3 unitarios (parser + utility fns).
11. [ ] Build `.msi` con `cargo tauri build`.
12. [ ] Push + tag v0.1.0 + entregar al auditor-ops.

## Estimación
~4-6 horas de ejecución del builder si skills ya están.

## Riesgos
- Si repo `panel-agentes` no existe → bloqueo esperando owner crear (gh auth).
- Si `claude --version` o `cargo` no están instalados en VPS-2 → escalar a orchestrator.

## Estado
borrador (esperando 01 y 02 aprobados)

## Comentarios de revisión
(vacío)
