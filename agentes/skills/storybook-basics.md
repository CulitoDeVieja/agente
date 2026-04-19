# storybook-basics

## Propósito
Documentar y desarrollar componentes React de forma aislada con Storybook.

## Cuándo usar
- Desarrollar UI components sin depender del estado completo de la app
- Documentar variantes y estados de componentes para el equipo
- Catálogo de componentes como design system

## Pasos

1. Instalar en proyecto Vite + React:
   ```bash
   npx storybook@latest init
   ```

2. Estructura de un Story:
   ```tsx
   // src/components/Button/Button.stories.tsx
   import type { Meta, StoryObj } from '@storybook/react';
   import { Button } from './Button';

   const meta: Meta<typeof Button> = {
     component: Button,
     title: 'UI/Button',
     tags: ['autodocs'],
     argTypes: {
       variant: { control: 'select', options: ['primary', 'secondary', 'ghost'] },
     },
   };

   export default meta;
   type Story = StoryObj<typeof Button>;

   export const Primary: Story = {
     args: { children: 'Guardar', variant: 'primary' },
   };

   export const Secondary: Story = {
     args: { children: 'Cancelar', variant: 'secondary' },
   };

   export const Disabled: Story = {
     args: { children: 'No disponible', variant: 'primary', disabled: true },
   };
   ```

3. Correr Storybook:
   ```bash
   npm run storybook
   # Abre http://localhost:6006
   ```

4. Integrar Tailwind en Storybook:
   ```ts
   // .storybook/preview.ts
   import '../src/index.css'; // importar el CSS con Tailwind
   ```

5. Buildear para publicar:
   ```bash
   npm run build-storybook
   # Genera storybook-static/ listo para hosting estático
   ```

## Ejemplo
El equipo de diseño revisa `Button`, `Card`, `Badge` en Storybook antes de aprobar el sprint.

## Errores comunes
- **Imports de contexto faltantes**: si el componente usa Context, wrapearlo en un Decorator global.
  ```ts
  // .storybook/preview.ts
  decorators: [(Story) => <QueryClientProvider client={qc}><Story /></QueryClientProvider>]
  ```
- **Tailwind no aplica estilos**: importar el CSS de Tailwind en `preview.ts`.
- **`autodocs` sin JSDoc**: el tag `autodocs` genera documentación automática; agregar comentarios al componente.
