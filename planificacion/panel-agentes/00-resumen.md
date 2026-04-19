# 00 — Resumen del proyecto panel-agentes

**Owner:** Antonio
**Creado:** 2026-04-19
**Escrito por:** orchestrator (consolidando pedido del owner)

## Objetivo
App nativa Windows (`.exe`) que lea el repo `agente` y muestre el progreso de cada agente y sus tareas.

## Motivación
Hoy el owner revisa progreso leyendo archivos a mano. Con la app lo ve de un vistazo.

## Scope v0.1
- Solo Windows.
- Lectura del repo local clonado (no remoto, no HTTP).
- 4 agentes en loop (orchestrator / skills-curator / builder / auditor-ops).
- Dashboard + vista detalle + botón refrescar.

## Fuera de scope (v0.2+)
- Crear tareas desde UI.
- Disparar ciclos remotos (SSH).
- Log de commits en vivo.
- Notificaciones push.

## Métricas de éxito
- `.msi` <15MB.
- App abre en <2s.
- Refresco manual <1s.
- Cero credenciales hardcodeadas.

## Restricciones
- Owner no programa. La app la codea builder.
- Presupuesto: 0 USD (todo dentro del plan Pro Max de Claude Code).

## Estado
aprobado (owner ya dió go)
