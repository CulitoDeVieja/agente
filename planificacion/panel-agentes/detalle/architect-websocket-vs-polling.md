# Decisión: websocket-vs-polling

**Decisión:** Ninguno. Refresh manual via botón ↻. Sin polling, sin websocket.

## Alternativas
- Polling 5s — quema CPU/disk IO sin feedback activo.
- Watch filesystem (notify crate) — complejo cross-platform, cache stale.
- WebSocket a servidor — no hay servidor.

## Justificación
Pull on demand coincide con trabajo humano. El agente remoto hace cambios, user pull cuando quiere ver.
