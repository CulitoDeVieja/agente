# MODO CELL!

Modo de absorción masiva de conocimiento. Orchestrator despliega **10 ciclos de evolución por agente** de golpe. Cada agente investiga, sintetiza, guarda en su `conocimiento/`. Al terminar todos, orchestrator coordina qué **queda en memoria** y qué se descarta.

## Flujo

1. **Despliegue:** orchestrator crea 30 tareas `<rol>-evo-<nnn>-<tema>.md` (10 por agente en loop).
2. **Ejecución:** cada agente toma sus 10 tareas, investiga web, guarda en `conocimiento/<rol>/YYYY-MM-DD-<tema>.md`, mueve a completado.
3. **Curaduría:** orchestrator lee los 30 archivos de conocimiento, clasifica en:
   - **Core:** queda permanente (tech que usamos ya).
   - **Backlog:** archivar en `conocimiento/<rol>/archivo/` (útil futuro).
   - **Descartar:** borrar (obsoleto, no aplica, mal investigado).
4. **Informe:** orchestrator escribe `conocimiento/INDICE.md` con lo Core.

## Cuándo disparar
Cuando el owner detecta que el stack se movió o entra tech nueva. Cada N semanas o por demanda.

## Regla dura
Cada archivo debe tener **fuente real**. Cero inventado. Si el agente no encuentra fuente, el archivo NO se guarda.
