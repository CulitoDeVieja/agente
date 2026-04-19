# EVOLUCION — PAUSADA

**Estado:** ⏸ pausada
**Desde:** 2026-04-19
**Por:** owner

## Regla
Mientras este archivo exista en la raíz del repo, **ningún agente ejecuta ciclo evolución**. Los ciclos que tocarían evolución se saltean y siguen con la cola normal.

## Cómo reactivar
Borrar este archivo:
```
git rm EVOLUCION-PAUSA.md && git commit -m "ORCH: reactivar EVOLUCION" && git push
```
