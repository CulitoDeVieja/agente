# react-testing-vitest

## Propósito
Configurar y escribir tests de componentes React con Vitest + Testing Library.

## Cuándo usar
Al agregar tests a componentes React en proyectos Vite.

## Pasos
1. Instalar: `npm install -D vitest @testing-library/react @testing-library/jest-dom jsdom`
2. En `vite.config.ts`:
   ```ts
   test: { environment: "jsdom", setupFiles: ["./src/test-setup.ts"] }
   ```
3. Crear `src/test-setup.ts`: `import "@testing-library/jest-dom"`
4. Escribir test:
   ```ts
   import { render, screen } from "@testing-library/react";
   test("muestra título", () => {
     render(<AgentCard name="builder" />);
     expect(screen.getByText("builder")).toBeInTheDocument();
   });
   ```
5. Correr: `npx vitest run`

## Ejemplo
```ts
import { fireEvent } from "@testing-library/react";
fireEvent.click(screen.getByRole("button", { name: /refresh/i }));
```

## Errores comunes
- `document is not defined` → falta `environment: "jsdom"` en config.
- Mocks de Tauri en tests → `vi.mock("@tauri-apps/api/core", () => ({ invoke: vi.fn() }))`.
- Tests lentos → usar `vitest --pool=forks` para aislar mejor.
