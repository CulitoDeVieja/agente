# persona-journey-writing

## Propósito
Escribir personas y user journeys que guíen decisiones de producto y diseño.

## Cuándo usar
Scout, al explorar usuarios del sistema o cuando el Architect necesita contexto humano antes de especificar.

## Pasos
1. Definir la persona en 4 campos: nombre ficticio, rol, objetivo principal, frustración principal.
2. Escribir el journey como pasos numerados: qué hace, qué piensa, qué siente en cada punto.
3. Marcar los pain points con `⚠️` y las oportunidades con `✓`.
4. Mantenerlo en ≤20 pasos; si es más largo, partir en sub-journeys.

## Ejemplo
```markdown
**Persona:** Marta, admin de startup, quiere onboardear empleados rápido.
**Frustración:** los procesos manuales le consumen el día.

**Journey: onboarding nuevo empleado**
1. Recibe mail de HR con fecha de ingreso → piensa: "¿dónde está el checklist?" ⚠️
2. Busca el doc en Drive → tarda 10 min en encontrarlo ⚠️
3. Completa el form de accesos → fluido ✓
```

## Errores comunes
- Persona sin frustración concreta → no sirve para tomar decisiones.
- Journey sin pain points → o es irreal o no se investigó suficiente.
- Más de 3 personas para un mismo producto → enfocarse en las 1-2 más relevantes.
