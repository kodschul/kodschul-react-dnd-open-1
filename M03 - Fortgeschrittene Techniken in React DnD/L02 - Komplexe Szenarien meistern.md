
# React DnD Schulung: Komplexe Szenarien meistern

In dieser Schulung lernen Sie, wie Sie komplexe Szenarien mit React DnD (Drag and Drop) meistern können. Der Fokus liegt auf der Implementierung von Drag & Drop über mehrere Ebenen sowie der Kombination verschiedener Typen von Draggable- und Drop-Zonen.

## Drag & Drop über mehrere Ebenen

### Herausforderung
In vielen Anwendungen ist es notwendig, Drag & Drop-Interaktionen über mehrere hierarchische Ebenen hinweg zu implementieren. Beispiele hierfür sind:
- Drag & Drop in verschachtelten Listen (z. B. Tree-Strukturen).
- Verschieben von Elementen zwischen Eltern- und Kindelementen.

### Implementierung

1. **Setup mit React DnD**:
   Installieren Sie die notwendigen Pakete:
   ```bash
   npm install react-dnd react-dnd-html5-backend
   ```

2. **Beispiel für eine verschachtelte Liste**:
   ```javascript
   import React from 'react';
   import { useDrag, useDrop, DndProvider } from 'react-dnd';
   import { HTML5Backend } from 'react-dnd-html5-backend';

   const ItemType = 'ITEM';

   const NestedItem = ({ item, moveItem }) => {
       const [, ref] = useDrag({
           type: ItemType,
           item: { id: item.id }
       });

       const [, drop] = useDrop({
           accept: ItemType,
           drop: (draggedItem) => moveItem(draggedItem.id, item.id),
       });

       return (
           <div ref={(node) => ref(drop(node))} style={{ padding: '8px', border: '1px solid #ccc', margin: '4px' }}>
               {item.name}
               {item.children && (
                   <div style={{ marginLeft: '16px' }}>
                       {item.children.map((child) => (
                           <NestedItem key={child.id} item={child} moveItem={moveItem} />
                       ))}
                   </div>
               )}
           </div>
       );
   };

   const App = () => {
       const [items, setItems] = React.useState([
           { id: 1, name: 'Item 1', children: [] },
           { id: 2, name: 'Item 2', children: [{ id: 3, name: 'Item 2.1', children: [] }] },
       ]);

       const moveItem = (draggedId, targetId) => {
           console.log(`Bewege ${draggedId} zu ${targetId}`);
       };

       return (
           <DndProvider backend={HTML5Backend}>
               {items.map((item) => (
                   <NestedItem key={item.id} item={item} moveItem={moveItem} />
               ))}
           </DndProvider>
       );
   };

   export default App;
   ```

3. **Wichtige Punkte**:
   - Verwenden Sie `useDrag` und `useDrop` an jedem Element in der Hierarchie.
   - Behalten Sie die Datenstruktur im Zustand und aktualisieren Sie diese entsprechend.

## Verschiedene Typen von Draggable- und Drop-Zonen kombinieren

### Herausforderung
Manchmal müssen unterschiedliche Typen von Draggable-Elementen mit spezifischen Drop-Zonen interagieren, z. B.:
- Ein bestimmter Elementtyp darf nur in einer bestimmten Zone abgelegt werden.
- Unterschiedliche Aktionen basierend auf dem Typ des gezogenen Elements.

### Implementierung

1. **Definition von Typen**:
   ```javascript
   const ItemTypes = {
       BOX: 'box',
       CIRCLE: 'circle',
   };
   ```

2. **Beispiel**:
   ```javascript
   import React from 'react';
   import { useDrag, useDrop, DndProvider } from 'react-dnd';
   import { HTML5Backend } from 'react-dnd-html5-backend';

   const DraggableBox = ({ id }) => {
       const [, ref] = useDrag({
           type: 'BOX',
           item: { id },
       });

       return <div ref={ref} style={{ width: '100px', height: '100px', backgroundColor: 'blue', margin: '10px' }}>Box {id}</div>;
   };

   const DraggableCircle = ({ id }) => {
       const [, ref] = useDrag({
           type: 'CIRCLE',
           item: { id },
       });

       return <div ref={ref} style={{ width: '100px', height: '100px', backgroundColor: 'green', borderRadius: '50%', margin: '10px' }}>Circle {id}</div>;
   };

   const DropZone = ({ accept, onDrop }) => {
       const [, ref] = useDrop({
           accept,
           drop: (item) => onDrop(item),
       });

       return <div ref={ref} style={{ width: '200px', height: '200px', border: '2px dashed #aaa', margin: '10px' }}>Drop Zone</div>;
   };

   const App = () => {
       const handleDrop = (item) => {
           console.log(`Element mit ID ${item.id} abgelegt.`);
       };

       return (
           <DndProvider backend={HTML5Backend}>
               <div style={{ display: 'flex' }}>
                   <DraggableBox id={1} />
                   <DraggableCircle id={2} />
               </div>
               <div style={{ display: 'flex' }}>
                   <DropZone accept="BOX" onDrop={handleDrop} />
                   <DropZone accept="CIRCLE" onDrop={handleDrop} />
               </div>
           </DndProvider>
       );
   };

   export default App;
   ```

3. **Wichtige Punkte**:
   - Legen Sie spezifische Typen für die Drag- und Drop-Zonen fest (`accept`).
   - Implementieren Sie individuelle Logik für jeden Typ von Element und Zone.

## Fazit

Mit React DnD können Sie selbst komplexe Drag & Drop-Szenarien einfach implementieren. Durch die Verwendung von Typen, hierarchischen Strukturen und spezifischen Drop-Zonen können Sie flexibel auf verschiedene Anforderungen eingehen und benutzerfreundliche Interaktionen gestalten.
