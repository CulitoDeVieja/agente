# tailwind-dark-mode

## Propósito
Implementar modo oscuro con Tailwind CSS v4 usando clase `dark` en el elemento raíz.

## Cuándo usar
Al agregar tema oscuro a una app React + Tailwind.

## Pasos
1. En CSS global: `@variant dark (&:where(.dark, .dark *));`
2. Toggle en React:
   ```ts
   const toggle = () => document.documentElement.classList.toggle("dark");
   ```
3. Usar clases `dark:`:
   ```tsx
   <div className="bg-white dark:bg-gray-900 text-black dark:text-white">
   ```
4. Persistir preferencia: `localStorage.setItem("theme", "dark")` y aplicar en `main.tsx`.

## Ejemplo
```tsx
useEffect(() => {
  const saved = localStorage.getItem("theme");
  if (saved === "dark") document.documentElement.classList.add("dark");
}, []);
```

## Errores comunes
- Parpadeo al cargar → aplicar clase en `<script>` inline en HTML antes de React hidrata.
- `dark:` no funciona → verificar que `@variant dark` está declarado en CSS.
- Sistema operativo prefiere dark pero app no lo respeta → leer `prefers-color-scheme` en el useEffect inicial.
