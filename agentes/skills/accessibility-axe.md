# accessibility-axe

## Propósito
Detectar problemas de accesibilidad automáticamente con axe-core en proyectos React.

## Cuándo usar
- Auditoría de accesibilidad durante el desarrollo
- Tests automáticos de a11y en CI
- Encontrar violaciones WCAG antes de que lleguen a producción

## Pasos

1. Instalar para desarrollo (overlay visual):
   ```bash
   npm install -D @axe-core/react
   ```

2. Activar en desarrollo:
   ```tsx
   // src/main.tsx
   if (import.meta.env.DEV) {
     const axe = await import('@axe-core/react');
     const React = await import('react');
     const ReactDOM = await import('react-dom');
     axe.default(React.default, ReactDOM.default, 1000);
   }
   ```
   Los problemas se muestran en la consola del browser con nivel de impacto.

3. Tests automatizados con Vitest + axe:
   ```bash
   npm install -D axe-core @testing-library/react @testing-library/jest-dom
   npm install -D vitest-axe
   ```

   ```ts
   import { render } from '@testing-library/react';
   import { axe, toHaveNoViolations } from 'vitest-axe';
   import { Button } from './Button';

   expect.extend(toHaveNoViolations);

   test('Button no tiene violaciones de a11y', async () => {
     const { container } = render(<Button>Guardar</Button>);
     const results = await axe(container);
     expect(results).toHaveNoViolations();
   });
   ```

4. Con Playwright (E2E):
   ```bash
   npm install -D @axe-core/playwright
   ```
   ```ts
   import AxeBuilder from '@axe-core/playwright';

   test('página sin violaciones críticas', async ({ page }) => {
     await page.goto('/');
     const results = await new AxeBuilder({ page }).analyze();
     expect(results.violations.filter(v => v.impact === 'critical')).toHaveLength(0);
   });
   ```

## Ejemplo
axe detecta: "Buttons must have discernible text" en un botón con solo ícono sin `aria-label`.

## Errores comunes
- **Falsos positivos**: algunos componentes de terceros violan reglas; usar `disableRules(['rule-id'])` selectivamente.
- **Solo a11y automática no es suficiente**: axe detecta ~30-40% de problemas; el resto requiere revisión manual.
- **No testear en prod**: `@axe-core/react` en producción impacta performance; usar solo en DEV.
