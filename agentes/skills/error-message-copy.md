# error-message-copy

## Propósito
Redactar mensajes de error que sean útiles, claros y que guíen al usuario a resolver el problema.

## Cuándo usar
- Errores de validación en formularios
- Errores de red o de servidor
- Operaciones fallidas (guardado, exportación, carga)

## Pasos

### Principios

1. **Qué pasó** — describir el problema en lenguaje humano (no códigos de error).
2. **Por qué pasó** — si es posible, dar contexto.
3. **Qué hacer** — indicar la acción para resolverlo.

### Fórmula
```
[Qué falló] + [Por qué (opcional)] + [Cómo solucionarlo]
```

### Ejemplos: mal vs bien

| Mal                           | Bien                                                  |
|-------------------------------|-------------------------------------------------------|
| "Error 500"                   | "No pudimos guardar. Intenta de nuevo en un momento." |
| "Invalid input"               | "El email no es válido. Usa el formato usuario@dominio.com." |
| "Operation failed"            | "No se pudo exportar el archivo. Verificá que tenés espacio disponible." |
| "Connection error"            | "Sin conexión. Conectate a internet y volvé a intentarlo." |
| "Authentication required"     | "Tu sesión expiró. Iniciá sesión nuevamente para continuar." |

### Errores de formulario

```tsx
// En campo de formulario:
<p role="alert" className="text-red-400 text-sm mt-1">
  {errors.email?.message ?? 'El email es requerido.'}
</p>
```

Mensajes recomendados por tipo:
- Campo vacío: "Este campo es requerido."
- Email inválido: "Ingresá un email válido (ej: usuario@mail.com)."
- Demasiado corto: "La contraseña debe tener al menos 8 caracteres."
- Ya existe: "Ese nombre ya está en uso. Elegí otro."

### Errores de red/servidor

```tsx
function getErrorMessage(err: unknown): string {
  if (err instanceof TypeError && err.message === 'Failed to fetch') {
    return 'Sin conexión. Verificá tu internet e intentá de nuevo.';
  }
  if (err instanceof Error) {
    return err.message || 'Algo salió mal. Intentá de nuevo.';
  }
  return 'Error inesperado. Si persiste, contactá soporte.';
}
```

## Ejemplo
Formulario de login: "La contraseña es incorrecta. ¿Olvidaste tu contraseña?" — nombra el problema y ofrece la salida.

## Errores comunes
- **Culpar al usuario**: "Has cometido un error" → "No pudimos procesar la solicitud".
- **Demasiado técnico**: stack traces o códigos HTTP crudos al usuario final.
- **Sin acción**: "Error al cargar" sin botón de reintentar deja al usuario bloqueado.
