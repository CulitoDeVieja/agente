# research-web

## Propósito
Investigar tecnologías, librerías o soluciones usando búsqueda web y documentación oficial.

## Cuándo usar
Scout o Skills-Curator, cuando se necesita evaluar opciones técnicas o entender una herramienta nueva.

## Pasos
1. Definir la pregunta concreta antes de buscar: "¿Qué librería X hace Y en ecosistema Z?"
2. Buscar primero en documentación oficial o repo GitHub.
3. Comparar máximo 3 opciones; más es parálisis.
4. Evaluar por: actividad del repo, tamaño del bundle/overhead, simplicidad de API.
5. Reportar en formato: opción recomendada + motivo + alternativas descartadas en 1 línea c/u.

## Ejemplo
```
Pregunta: ¿Qué usar para validación de schemas en Node.js?
Recomendado: zod — TS-first, API fluida, activo.
Descartados: joi (más verboso), yup (más lento en compilación).
```

## Errores comunes
- Buscar sin pregunta definida → produce ruido, no respuesta.
- Elegir la opción más popular sin evaluar fit → popularidad ≠ correcto para el caso.
- Research sin conclusión accionable → siempre terminar con recomendación concreta.
