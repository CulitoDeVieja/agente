# notification-levels

## Propósito
Definir y aplicar correctamente los niveles de notificación (info, warning, error, success) en la UI.

## Cuándo usar
- Diseñar el sistema de feedback de la app
- Decidir qué nivel usar para cada tipo de mensaje
- Asegurar consistencia en el trato de notificaciones

## Pasos

### Niveles y cuándo usar cada uno

| Nivel   | Color       | Cuándo usar                                                       |
|---------|-------------|-------------------------------------------------------------------|
| info    | Azul/neutro | Información útil no urgente, tips, avisos contextuales            |
| success | Verde       | Operación completada exitosamente                                  |
| warning | Amarillo    | Situación que requiere atención pero no bloquea al usuario        |
| error   | Rojo        | Operación fallida o estado que requiere acción inmediata          |

### Ejemplos por nivel

**Info:**
- "Tus datos se sincronizan cada 30 minutos"
- "Esta función está en beta"
- "Tenés 3 tareas vencidas esta semana"

**Success:**
- "Tarea guardada"
- "Exportación completada"
- "Contraseña actualizada"

**Warning:**
- "Tu plan gratuito alcanza el límite (80/100 tareas)"
- "No tenés conexión — los cambios se guardarán localmente"
- "Esta acción no se puede deshacer"

**Error:**
- "No se pudo guardar. Intentá de nuevo."
- "Error de conexión — verificá tu internet"
- "Sesión expirada. Iniciá sesión nuevamente."

### Componente de alerta

```tsx
type AlertLevel = 'info' | 'success' | 'warning' | 'error';

const styles: Record<AlertLevel, string> = {
  info:    'bg-blue-500/10 border-blue-500/30 text-blue-300',
  success: 'bg-green-500/10 border-green-500/30 text-green-300',
  warning: 'bg-yellow-500/10 border-yellow-500/30 text-yellow-300',
  error:   'bg-red-500/10 border-red-500/30 text-red-300',
};

function Alert({ level, message }: { level: AlertLevel; message: string }) {
  return (
    <div role={level === 'error' ? 'alert' : 'status'}
         className={`rounded-lg border px-4 py-3 text-sm ${styles[level]}`}>
      {message}
    </div>
  );
}
```

### Reglas de uso

1. **No usar error para warnings**: reservar el rojo para lo que realmente falló.
2. **Un mensaje a la vez por área**: apilar múltiples mensajes confunde.
3. **Error debe tener acción**: si algo falla, dar al usuario algo que hacer.
4. **Warning no bloquea**: el usuario puede ignorar un warning y continuar.

## Ejemplo
Storage casi lleno → warning (puede continuar). Storage lleno y guardado falló → error (necesita acción).

## Errores comunes
- **Usar error para todo**: si se abusa del rojo, pierde urgencia semántica.
- **Warning sin contexto**: "Atención" sin explicar qué requiere atención es inútil.
- **Info sin dismissal**: mensajes informativos persistentes se vuelven ruido; hacerlos cerrables.
