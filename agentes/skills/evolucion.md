# evolucion

## Propósito
Que cada agente crezca su propia base de conocimiento investigando activamente. Evita reinventar y trae tech nueva al sistema.

## ⏸ Chequeo de pausa
**Antes de ejecutar un ciclo de evolución**, verificar si existe `EVOLUCION-PAUSA.md` en la raíz del repo:
```
test -f EVOLUCION-PAUSA.md && echo "PAUSADO, saltear evolucion"
```
Si está pausada → saltar a la cola de tareas normales.

## Cuándo usar
Cada 10 ciclos, en vez de tomar una tarea de la cola, dedicar 1 ciclo a investigar — solo si NO está pausada.

## Pasos del ciclo evolución

1. `git pull --rebase` del repo agente.
2. Elegir tema relevante a tu rol (ver lista abajo).
3. Buscar web (WebSearch / WebFetch) información concreta: librería, patrón, CVE, release, paper corto.
4. Sintetizar en archivo: `conocimiento/<tu-rol>/YYYY-MM-DD-<tema-slug>.md`
   - Formato: TL;DR (3 líneas) + detalles + fuente + aplicabilidad a nuestro proyecto.
5. Commit + push inmediato: `<rol>: evolucion — <tema>`.

## Temas de investigación por rol

### orchestrator
- Patrones de coordinación multi-agente.
- Sistemas de colas distribuidas (git-based, queue libs).
- Tooling de observabilidad para agentes.

### skills-curator
- Nuevas librerías React/TS/Tauri/Rust.
- Patrones de skills reusables que estén de moda.
- Cambios breaking en versiones recientes.

### builder
- Snippets útiles (hooks custom, Rust idioms).
- Herramientas DX nuevas (bundlers, testing).
- Librerías que reemplazan deps pesadas.

### auditor-ops
- Tools de audit (eslint plugins, cargo audit, CVE dbs).
- Best practices de release Windows (code signing, SmartScreen).
- Métricas de performance (Lighthouse, bundle analyzers).

## Regla dura — NO DUPLICAR

Antes de escribir skill nueva, componente nuevo, tarea nueva:
```
grep -r "<palabra-clave>" conocimiento/<tu-rol>/
```
Si hay resultado → reusá o referencia. NO dupliques trabajo.

## Entregable del ciclo evolución

Un archivo markdown ≤40 líneas con:
- **TL;DR** (3 líneas máximo)
- **Detalles** (qué es, para qué sirve, ejemplo mínimo)
- **Fuente** (URL o referencia concreta, NO inventada)
- **Aplicabilidad** (¿lo usamos en panel-agentes o futuros proyectos? sí/no + por qué)

## Errores comunes

- Investigar sin foco → salir del tema, perder tiempo. Elegí UN tema por ciclo.
- Fuentes inventadas → PROHIBIDO. Si no encontrás fuente real, no escribas el archivo.
- Copiar código completo sin entenderlo → referenciá + nota "revisar en detalle".
- Guardar duplicados → grep antes.
