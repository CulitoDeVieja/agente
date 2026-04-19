# compose-panel-layout

## Propósito
Componer un layout de paneles (sidebar + main + detail) en React con Tailwind CSS.

## Cuándo usar
- Apps de productividad con navegación lateral y área principal
- Layouts tipo "three-pane" (Gmail, Notion, Linear)
- Paneles redimensionables o colapsables

## Pasos

1. Layout base de tres columnas:
   ```tsx
   function AppLayout() {
     return (
       <div className="flex h-screen w-full overflow-hidden bg-surface">
         <Sidebar />
         <MainPane />
         <DetailPane />
       </div>
     );
   }
   ```

2. Sidebar colapsable:
   ```tsx
   function Sidebar() {
     const [collapsed, setCollapsed] = useState(false);
     return (
       <aside className={`flex flex-col border-r border-border transition-all duration-200
         ${collapsed ? 'w-14' : 'w-60'}`}
       >
         <button onClick={() => setCollapsed(p => !p)} className="p-3">
           {collapsed ? '→' : '←'}
         </button>
         {!collapsed && <NavItems />}
       </aside>
     );
   }
   ```

3. Panel principal con scroll propio:
   ```tsx
   function MainPane() {
     return (
       <main className="flex flex-1 flex-col overflow-hidden">
         <TopBar />
         <div className="flex-1 overflow-y-auto p-4">
           <TaskList />
         </div>
       </main>
     );
   }
   ```

4. Panel de detalle condicional:
   ```tsx
   function DetailPane() {
     const { selectedTask } = useTaskStore();
     if (!selectedTask) return null;
     return (
       <aside className="w-80 border-l border-border overflow-y-auto">
         <TaskDetail task={selectedTask} />
       </aside>
     );
   }
   ```

5. Redimensionable con `react-resizable-panels`:
   ```bash
   npm install react-resizable-panels
   ```
   ```tsx
   import { Panel, PanelGroup, PanelResizeHandle } from 'react-resizable-panels';

   <PanelGroup direction="horizontal">
     <Panel defaultSize={20} minSize={10}><Sidebar /></Panel>
     <PanelResizeHandle className="w-1 bg-border hover:bg-brand-500" />
     <Panel><MainPane /></Panel>
   </PanelGroup>
   ```

## Ejemplo
Layout tipo Linear: sidebar 240px fija, main flexible, detail panel 320px que aparece al seleccionar una issue.

## Errores comunes
- **Overflow en el contenedor root**: el `h-screen overflow-hidden` debe estar en el div raíz.
- **Scroll en la página entera**: cada panel debe tener su propio `overflow-y-auto` para scroll independiente.
- **Detail panel empuja el layout**: usar `absolute` o `fixed` si no se quiere que desplace el main.
