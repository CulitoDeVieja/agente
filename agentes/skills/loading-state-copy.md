# loading-state-copy

## Propósito
Redactar mensajes de carga que transmitan progreso y mantengan al usuario informado.

## Cuándo usar
- Operaciones async que tardan más de 300ms
- Carga inicial de la app o de una sección
- Exportaciones, importaciones, sincronizaciones

## Pasos

### Principios

1. **Informar qué se está cargando** — no solo "Cargando..."
2. **Transmitir progreso** cuando sea posible (%, steps)
3. **Mantener la interfaz utilizable** en lo posible (skeleton > spinner)
4. **Gestionar expectativas** si va a tardar

### Mensajes por contexto

| Contexto                   | Mensaje recomendado                          |
|----------------------------|----------------------------------------------|
| Carga inicial              | "Cargando tu espacio de trabajo..."          |
| Sincronización             | "Sincronizando tareas..."                    |
| Guardando                  | "Guardando cambios..."                       |
| Exportando                 | "Generando tu archivo PDF..."                |
| Importando datos           | "Importando 247 tareas..."                   |
| Carga lenta (>5s)          | "Esto puede tardar unos segundos..."         |
| Con progreso               | "Procesando 3 de 10 archivos..."             |

### Componente de loading con mensaje

```tsx
interface LoadingProps {
  message?: string;
  progress?: number; // 0-100
}

function LoadingState({ message = 'Cargando...', progress }: LoadingProps) {
  return (
    <div className="flex flex-col items-center gap-3 py-12" role="status">
      <div className="h-6 w-6 animate-spin rounded-full border-2 border-brand-500 border-t-transparent" />
      <p className="text-sm text-text-muted">{message}</p>
      {progress !== undefined && (
        <div className="w-48">
          <div className="h-1.5 rounded-full bg-surface-alt">
            <div
              className="h-1.5 rounded-full bg-brand-500 transition-all"
              style={{ width: `${progress}%` }}
            />
          </div>
          <p className="mt-1 text-center text-xs text-text-muted">{progress}%</p>
        </div>
      )}
      <span className="sr-only">{message}</span>
    </div>
  );
}
```

### Skeleton loaders (preferible al spinner para listas)

```tsx
function TaskSkeleton() {
  return (
    <div className="flex gap-3 p-3 animate-pulse">
      <div className="h-4 w-4 rounded bg-surface-alt" />
      <div className="flex-1 space-y-2">
        <div className="h-3 w-3/4 rounded bg-surface-alt" />
        <div className="h-3 w-1/2 rounded bg-surface-alt" />
      </div>
    </div>
  );
}
```

## Ejemplo
Exportación de PDF: spinner + "Generando tu archivo... esto puede tardar unos segundos." — el usuario sabe que es normal que tarde.

## Errores comunes
- **"Cargando..." genérico para todo**: ser específico reduce la ansiedad del usuario.
- **Spinner sin texto**: siempre acompañar con texto (o `aria-label`) para accesibilidad.
- **No mostrar loading en operaciones rápidas (<300ms)**: el flash de spinner es peor que nada; usar un delay de 300ms antes de mostrar.
