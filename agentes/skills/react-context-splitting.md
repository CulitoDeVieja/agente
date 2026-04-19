# react-context-splitting

## Propósito
Dividir un Context grande de React en múltiples contextos más pequeños para evitar re-renders innecesarios.

## Cuándo usar
- Un Context tiene estado que cambia frecuentemente junto a datos estáticos
- Componentes se re-renderizan aunque no usen los datos que cambiaron
- El profiler muestra renders excesivos en componentes hijos

## Pasos

1. Identificar qué partes del contexto cambian juntas y cuáles son estables.

2. Separar en contextos distintos:
   ```ts
   // contexts/ThemeContext.tsx (cambia raramente)
   const ThemeContext = createContext<Theme | null>(null);

   // contexts/UserContext.tsx (cambia al login/logout)
   const UserContext = createContext<User | null>(null);

   // contexts/NotificationsContext.tsx (cambia frecuentemente)
   const NotificationsContext = createContext<Notification[]>([]);
   ```

3. Componer los providers en el árbol:
   ```tsx
   function AppProviders({ children }: { children: React.ReactNode }) {
     return (
       <ThemeProvider>
         <UserProvider>
           <NotificationsProvider>
             {children}
           </NotificationsProvider>
         </UserProvider>
       </ThemeProvider>
     );
   }
   ```

4. Separar state y dispatch en Context de alta frecuencia:
   ```ts
   const CountStateContext = createContext(0);
   const CountDispatchContext = createContext<Dispatch<Action>>(() => {});
   ```
   Así los componentes que solo llaman dispatch no se re-renderizan cuando el state cambia.

## Ejemplo
Un dashboard con tema global (raro), usuario (ocasional) y notificaciones en tiempo real usa tres contexts separados.

## Errores comunes
- **Demasiados contextos**: no atomizar más de lo necesario; usar Zustand si hay más de 4-5.
- **Memoizar el value del provider**: sin `useMemo`, el objeto value se recrea en cada render.
  ```tsx
  const value = useMemo(() => ({ user, login, logout }), [user]);
  ```
