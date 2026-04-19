# Plan: componente-dashboard

## Objetivo
Vista principal `/` con 4 `AgentCard` y header con badge modo master.

## Layout
```
Header: "PANEL AGENTES" + [MODO MASTER ●] si activo + [↻ Refrescar]
Grid 2x2: AgentCard x4
```

## Pasos
1. Leer `agents` del context
2. Renderizar grid con `AgentCard` por cada agente
3. Mostrar badge `MODO MASTER` si `modoMaster === true`
4. `RefreshButton` en header

## Tests planeados
- [ ] Renderiza 4 cards
- [ ] Badge visible cuando modoMaster true
