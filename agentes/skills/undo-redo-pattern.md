# undo-redo-pattern

## Propósito
Implementar undo/redo en React para permitir al usuario revertir y rehacer acciones.

## Cuándo usar
- Editores de texto, notas, diagramas
- Listas ordenables con acciones reversibles
- Formularios complejos donde el usuario puede cometer errores

## Pasos

1. Patrón de historial con `useReducer`:
   ```ts
   interface HistoryState<T> {
     past: T[];
     present: T;
     future: T[];
   }

   type HistoryAction<T> =
     | { type: 'SET'; payload: T }
     | { type: 'UNDO' }
     | { type: 'REDO' };

   function historyReducer<T>(
     state: HistoryState<T>,
     action: HistoryAction<T>
   ): HistoryState<T> {
     switch (action.type) {
       case 'SET':
         return {
           past: [...state.past, state.present],
           present: action.payload,
           future: [],
         };
       case 'UNDO':
         if (!state.past.length) return state;
         return {
           past: state.past.slice(0, -1),
           present: state.past[state.past.length - 1],
           future: [state.present, ...state.future],
         };
       case 'REDO':
         if (!state.future.length) return state;
         return {
           past: [...state.past, state.present],
           present: state.future[0],
           future: state.future.slice(1),
         };
     }
   }
   ```

2. Hook de historial:
   ```ts
   function useHistory<T>(initialState: T) {
     const [state, dispatch] = useReducer(historyReducer<T>, {
       past: [],
       present: initialState,
       future: [],
     });

     return {
       state: state.present,
       set: (value: T) => dispatch({ type: 'SET', payload: value }),
       undo: () => dispatch({ type: 'UNDO' }),
       redo: () => dispatch({ type: 'REDO' }),
       canUndo: state.past.length > 0,
       canRedo: state.future.length > 0,
     };
   }
   ```

3. Uso en un componente:
   ```tsx
   function NoteEditor() {
     const { state: text, set, undo, redo, canUndo, canRedo } = useHistory('');

     useHotkeys('ctrl+z, meta+z', undo);
     useHotkeys('ctrl+shift+z, meta+shift+z', redo);

     return (
       <>
         <div className="flex gap-2">
           <button onClick={undo} disabled={!canUndo}>↩ Deshacer</button>
           <button onClick={redo} disabled={!canRedo}>↪ Rehacer</button>
         </div>
         <textarea value={text} onChange={e => set(e.target.value)} />
       </>
     );
   }
   ```

4. Limitar el tamaño del historial:
   ```ts
   case 'SET':
     const MAX_HISTORY = 50;
     return {
       past: [...state.past.slice(-MAX_HISTORY), state.present],
       present: action.payload,
       future: [],
     };
   ```

## Ejemplo
Editor de notas: `Ctrl+Z` deshace el último cambio de texto; `Ctrl+Shift+Z` lo rehace.

## Errores comunes
- **Historial sin límite**: con muchos cambios, el array `past` crece sin límite; aplicar `MAX_HISTORY`.
- **Guardar en cada keystroke**: en inputs de texto, debounce antes de guardar en historial para no llenar de un carácter por entrada.
- **Undo de acciones async**: el undo de acciones que ya persisten en el servidor requiere una llamada inversa a la API.
