# test-e2e-playwright

## Propósito
Escribir y correr tests end-to-end con Playwright para apps web y apps Tauri 2.

## Cuándo usar
- Verificar flujos críticos de usuario (login, crear tarea, exportar)
- Regresiones en CI antes de cada release
- Testing de UI que no se puede cubrir con unit tests

## Pasos

1. Instalar:
   ```bash
   npm install -D @playwright/test
   npx playwright install
   ```

2. Configurar `playwright.config.ts`:
   ```ts
   import { defineConfig } from '@playwright/test';

   export default defineConfig({
     testDir: './tests/e2e',
     use: {
       baseURL: 'http://localhost:5173',
       screenshot: 'only-on-failure',
       video: 'retain-on-failure',
     },
     webServer: {
       command: 'npm run dev',
       url: 'http://localhost:5173',
       reuseExistingServer: !process.env.CI,
     },
   });
   ```

3. Escribir un test:
   ```ts
   // tests/e2e/tasks.spec.ts
   import { test, expect } from '@playwright/test';

   test('crear una tarea nueva', async ({ page }) => {
     await page.goto('/');
     await page.getByRole('button', { name: 'Nueva tarea' }).click();
     await page.getByLabel('Título').fill('Mi tarea de prueba');
     await page.getByRole('button', { name: 'Guardar' }).click();
     await expect(page.getByText('Mi tarea de prueba')).toBeVisible();
   });
   ```

4. Para Tauri 2 con `tauri-driver`:
   ```bash
   cargo install tauri-driver
   npm install -D webdriverio
   ```
   Usar WebdriverIO como cliente y `tauri-driver` como servidor WebDriver.

5. Correr tests:
   ```bash
   npx playwright test              # todos
   npx playwright test --ui         # modo visual
   npx playwright test tasks.spec   # uno solo
   ```

## Ejemplo
En CI, el test de "crear tarea" corre contra el build de producción y falla si el botón no aparece.

## Errores comunes
- **Selectores frágiles**: preferir `getByRole`, `getByLabel` sobre `.className` o XPath.
- **Race conditions**: usar `await expect(...).toBeVisible()` en lugar de `waitForTimeout`.
- **Tests interdependientes**: cada test debe ser independiente; limpiar estado entre tests.
