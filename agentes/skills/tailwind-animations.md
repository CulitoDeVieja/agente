# tailwind-animations

## Propósito
Agregar animaciones y transiciones CSS con las utilidades de Tailwind y keyframes custom.

## Cuándo usar
- Transiciones de estado (hover, focus, active)
- Animaciones de entrada/salida de elementos
- Loaders, spinners, pulsos

## Pasos

1. Transiciones básicas con Tailwind:
   ```tsx
   <button className="bg-brand-500 transition-colors duration-200 hover:bg-brand-600">
     Guardar
   </button>

   <div className="transform transition-transform duration-300 hover:scale-105">
     Card
   </div>
   ```

2. Clases de animación predefinidas:
   ```tsx
   <div className="animate-spin">🔄</div>
   <div className="animate-ping">⬤</div>
   <div className="animate-pulse bg-gray-200 h-4 w-32 rounded" />
   <div className="animate-bounce">↓</div>
   ```

3. Keyframes custom en Tailwind v4:
   ```css
   @theme {
     --animate-fade-in: fade-in 0.3s ease-out;
     --animate-slide-up: slide-up 0.4s cubic-bezier(0.16, 1, 0.3, 1);
   }

   @keyframes fade-in {
     from { opacity: 0; }
     to   { opacity: 1; }
   }

   @keyframes slide-up {
     from { opacity: 0; transform: translateY(12px); }
     to   { opacity: 1; transform: translateY(0); }
   }
   ```

4. Usar la animación custom:
   ```tsx
   <div className="animate-fade-in">Aparece suavemente</div>
   <div className="animate-slide-up">Sube desde abajo</div>
   ```

5. Reducir movimiento (accesibilidad):
   ```css
   @media (prefers-reduced-motion: reduce) {
     *, *::before, *::after { animation-duration: 0.01ms !important; }
   }
   ```

## Ejemplo
Modal que aparece con `animate-fade-in` y cuyos botones tienen `transition-colors duration-150`.

## Errores comunes
- **`transition` sin propiedad específica**: `transition-all` puede ser costoso; preferir `transition-colors` o `transition-transform`.
- **Animación no termina**: verificar que no haya `animation-fill-mode` conflictivo.
- **Sin `will-change`**: para animaciones complejas, `will-change-transform` mejora el rendimiento.
