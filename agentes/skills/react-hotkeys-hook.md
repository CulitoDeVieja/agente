# react-hotkeys-hook

## Propósito
Registrar atajos de teclado en componentes React con react-hotkeys-hook de forma declarativa.

## Cuándo usar
- Atajos dentro de la app (no globales de sistema)
- Acciones de teclado contextuales según el componente activo
- Paletas de comandos, shortcuts de editor, navegación por teclado

## Pasos

1. Instalar:
   ```bash
   npm install react-hotkeys-hook
   ```

2. Uso básico:
   ```ts
   import { useHotkeys } from 'react-hotkeys-hook';

   function Editor() {
     useHotkeys('ctrl+s, meta+s', (e) => {
       e.preventDefault();
       handleSave();
     });

     useHotkeys('ctrl+z', () => undo());
     useHotkeys('ctrl+shift+z', () => redo());

     return <div>Editor</div>;
   }
   ```

3. Opciones útiles:
   ```ts
   useHotkeys(
     'ctrl+k',
     () => openCommandPalette(),
     {
       preventDefault: true,       // evitar comportamiento del browser
       enableOnFormTags: false,    // no activar dentro de inputs
       scopes: 'editor',           // namespacing de scopes
     }
   );
   ```

4. Activar atajos dentro de inputs con `enableOnFormTags`:
   ```ts
   useHotkeys('escape', () => clearInput(), {
     enableOnFormTags: ['INPUT', 'TEXTAREA'],
   });
   ```

5. Mapa de atajos para documentación:
   ```ts
   const shortcuts = [
     { keys: 'ctrl+s', action: 'Guardar' },
     { keys: 'ctrl+k', action: 'Abrir paleta' },
   ];
   ```

## Ejemplo
Presionar `Ctrl+K` abre el command palette sin importar dónde esté el foco en la app.

## Errores comunes
- **Atajo no se activa en inputs**: por defecto está deshabilitado; usar `enableOnFormTags`.
- **Conflicto entre componentes**: si dos componentes registran el mismo atajo, ambos se disparan; usar scopes.
- **Cleanup innecesario**: el hook limpia solo cuando el componente se desmonta.
