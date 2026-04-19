# Plan: componente-agent-card

## Objetivo
Componente `AgentCard.tsx` — card clickeable por agente en dashboard.

## Props
```ts
type AgentCardProps = { agent: AgentSnapshot; onClick: () => void; }
```

## Layout
- Header: rol + badge estado (✅/⏳)
- Body: contadores P/E/C
- Click navega a `/agent/:role`

## Tests planeados
- [ ] Renderiza rol correctamente
- [ ] Badge verde para `✅ activo`, amarillo para `⏳`
- [ ] onClick dispara callback
