# yup-vs-zod

## Propósito
Decidir cuándo usar Yup y cuándo usar Zod para validación de schemas en proyectos JS/TS.

## Cuándo usar
Consultar esta guía al elegir librería de validación para un nuevo módulo o proyecto.

## Pasos

### Comparación rápida

| Criterio              | Zod                        | Yup                         |
|-----------------------|----------------------------|-----------------------------|
| TypeScript-first      | Sí (tipos inferidos)       | Parcial (requiere casteo)   |
| Tamaño bundle         | ~14 KB                     | ~30 KB                      |
| Inferencia de tipos   | Automática con `z.infer`   | Manual o con `InferType`    |
| Async validation      | Sí (`.refine` async)       | Sí (nativo)                 |
| Ecosistema React      | react-hook-form, tRPC      | Formik, react-hook-form     |
| Curva de aprendizaje  | Baja                       | Baja                        |
| Parse vs validate     | Parse + transform          | Validate only               |

### Elegir Zod cuando:
- El proyecto es TypeScript puro y se quiere inferencia automática de tipos
- Se usa con react-hook-form (resolver oficial)
- Se necesita transformar datos al parsear (coerción de tipos)
- Se usa tRPC o se comparte schema entre front y back

### Elegir Yup cuando:
- El proyecto ya usa Formik (integración nativa)
- El equipo conoce Yup y migrar no aporta valor
- Se necesita validación asíncrona compleja con contexto externo

## Ejemplo

**Zod:**
```ts
const schema = z.object({ age: z.number().min(18) });
type User = z.infer<typeof schema>; // automático
```

**Yup:**
```ts
const schema = yup.object({ age: yup.number().min(18).required() });
type User = yup.InferType<typeof schema>; // manual
```

## Errores comunes
- **Usar Yup en proyecto nuevo TypeScript**: perder la inferencia automática sin beneficio.
- **Mezclar ambas librerías**: genera inconsistencia; elegir una y unificar.
- **Zod para validación asíncrona simple**: `.refine(async () => ...)` funciona bien pero agregar contexto es más verboso que en Yup.
