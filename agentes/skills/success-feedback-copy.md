# success-feedback-copy

## Propósito
Redactar mensajes de éxito concisos y útiles que confirmen la acción sin ser redundantes.

## Cuándo usar
- Guardado de datos
- Operaciones completadas (exportar, enviar, eliminar)
- Configuración actualizada

## Pasos

### Principios

1. **Confirmar qué se hizo** — no solo "¡Éxito!"
2. **Ser específico** — incluir el nombre del objeto si es posible
3. **Ser breve** — el éxito no necesita explicación larga
4. **Ofrecer siguiente paso** cuando aporte valor

### Mensajes por contexto

| Acción                  | Mensaje recomendado                              |
|-------------------------|--------------------------------------------------|
| Guardar cambios         | "Cambios guardados"                              |
| Crear ítem              | "Tarea creada"                                   |
| Eliminar ítem           | "Tarea eliminada" + [Deshacer]                   |
| Exportar archivo        | "Archivo exportado a Descargas"                  |
| Copiar al portapapeles  | "Copiado"                                        |
| Enviar formulario       | "Tu mensaje fue enviado"                         |
| Sincronizar             | "Todo al día"                                    |
| Cambiar contraseña      | "Contraseña actualizada correctamente"           |

### Toasts de éxito

```ts
// Directo y específico:
toast.success('Tarea guardada');
toast.success(`"${task.title}" movida a Completadas`);

// Con nombre del archivo:
toast.success(`Exportado: informe-abril.pdf`);

// Con acción siguiente (usando toast custom):
toast.custom((t) => (
  <div className="flex items-center gap-3">
    <CheckIcon className="text-green-400" />
    <span>Tarea eliminada</span>
    <button onClick={() => { undoDelete(); toast.dismiss(t.id); }}>
      Deshacer
    </button>
  </div>
), { duration: 5000 });
```

### Feedback inline (sin toast)

```tsx
function SaveButton({ saved }: { saved: boolean }) {
  return (
    <button className="flex items-center gap-2">
      {saved ? (
        <>
          <CheckIcon className="text-green-400 h-4 w-4" />
          <span className="text-green-400">Guardado</span>
        </>
      ) : (
        'Guardar'
      )}
    </button>
  );
}
```

### Tono
- Pasado simple: "Guardado", "Eliminado", "Enviado" — no "¡Guardado exitosamente!"
- Sin exclamaciones excesivas: una confirmación tranquila genera más confianza que una celebración
- Específico > genérico: "Archivo PDF exportado" > "Operación completada"

## Ejemplo
Eliminar una tarea: toast "Tarea eliminada" con botón "Deshacer" visible 5 segundos — confirma y da escape.

## Errores comunes
- **Celebraciones exageradas**: "¡Fantástico! ¡Lo lograste!" en acciones rutinarias cansa al usuario.
- **Sin feedback en acciones críticas**: guardar sin confirmación genera incertidumbre.
- **Toast muy corto**: para acciones con "Deshacer", el toast debe durar al menos 4-5 segundos.
