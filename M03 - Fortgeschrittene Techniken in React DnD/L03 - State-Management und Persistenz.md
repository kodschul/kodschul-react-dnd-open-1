
# React DnD Schulung: State-Management und Persistenz

In dieser Schulung werden fortgeschrittene Techniken im Zusammenhang mit Drag & Drop (DnD) in React behandelt. Der Schwerpunkt liegt auf dem effektiven State-Management und der Synchronisierung von Drag & Drop-Daten mit Redux.

## Änderungen im React State handhaben

Der React State ist der zentrale Speicherort für die Zustände einer Komponente. Beim Implementieren von Drag & Drop ist es wichtig, die Änderungen im State effizient zu verwalten.

### Best Practices:
1. **Vermeidung von Mutationen**:
   - Der State sollte niemals direkt verändert werden.
   - Verwenden Sie Funktionen wie `setState` oder Redux-Reducer.

2. **Verwendung von State-Updatelogik**:
   - Aktualisieren Sie den State basierend auf der Benutzerinteraktion (z. B. Drag-Ereignisse).
   - Beispiel:
     ```javascript
     const [items, setItems] = useState(["Item 1", "Item 2", "Item 3"]);

     const handleDrop = (dragIndex, hoverIndex) => {
         const updatedItems = [...items];
         const [draggedItem] = updatedItems.splice(dragIndex, 1);
         updatedItems.splice(hoverIndex, 0, draggedItem);
         setItems(updatedItems);
     };
     ```

3. **Optimierung der Performance**:
   - Vermeiden Sie unnötige State-Updates.
   - Nutzen Sie `React.memo` oder `useMemo` für die Optimierung.

### Beispiel für State-Handling:
```javascript
import React, { useState } from "react";
import { DndProvider, useDrag, useDrop } from "react-dnd";
import { HTML5Backend } from "react-dnd-html5-backend";

const DraggableItem = ({ item, index, moveItem }) => {
    const [, ref] = useDrag({
        type: "ITEM",
        item: { index },
    });

    const [, drop] = useDrop({
        accept: "ITEM",
        hover: (draggedItem) => {
            if (draggedItem.index !== index) {
                moveItem(draggedItem.index, index);
                draggedItem.index = index;
            }
        },
    });

    return (
        <div ref={(node) => ref(drop(node))}>
            {item}
        </div>
    );
};

const DragDropContainer = () => {
    const [items, setItems] = useState(["Item A", "Item B", "Item C"]);

    const moveItem = (dragIndex, hoverIndex) => {
        const updatedItems = [...items];
        const [movedItem] = updatedItems.splice(dragIndex, 1);
        updatedItems.splice(hoverIndex, 0, movedItem);
        setItems(updatedItems);
    };

    return (
        <DndProvider backend={HTML5Backend}>
            {items.map((item, index) => (
                <DraggableItem
                    key={item}
                    item={item}
                    index={index}
                    moveItem={moveItem}
                />
            ))}
        </DndProvider>
    );
};

export default DragDropContainer;
```

## Drag & Drop-Daten mit Redux synchronisieren

Die Synchronisierung von Drag & Drop-Daten mit Redux bietet eine zentrale und einheitliche Verwaltung des Anwendungszustands. Dies ist besonders nützlich für komplexe Anwendungen mit mehreren Komponenten.

### Schritte zur Integration:
1. **State in Redux speichern**:
   - Speichern Sie die Drag & Drop-Daten im Redux-Store.

2. **Reducer zur Aktualisierung des States**:
   - Schreiben Sie einen Reducer, der die Drag & Drop-Änderungen verarbeitet.
   ```javascript
   const itemsReducer = (state = [], action) => {
       switch (action.type) {
           case "MOVE_ITEM":
               const { dragIndex, hoverIndex } = action.payload;
               const updatedItems = [...state];
               const [movedItem] = updatedItems.splice(dragIndex, 1);
               updatedItems.splice(hoverIndex, 0, movedItem);
               return updatedItems;
           default:
               return state;
       }
   };
   ```

3. **Actions definieren**:
   - Erstellen Sie eine Action zum Verschieben von Elementen.
   ```javascript
   export const moveItem = (dragIndex, hoverIndex) => ({
       type: "MOVE_ITEM",
       payload: { dragIndex, hoverIndex },
   });
   ```

4. **Drag & Drop mit Redux verbinden**:
   - Verwenden Sie `useDispatch` und `useSelector`, um den Redux-Store in Ihre Komponenten zu integrieren.
   ```javascript
   import { useDispatch, useSelector } from "react-redux";
   import { moveItem } from "./actions";

   const DraggableItem = ({ item, index }) => {
       const dispatch = useDispatch();

       const [, ref] = useDrag({
           type: "ITEM",
           item: { index },
       });

       const [, drop] = useDrop({
           accept: "ITEM",
           hover: (draggedItem) => {
               if (draggedItem.index !== index) {
                   dispatch(moveItem(draggedItem.index, index));
                   draggedItem.index = index;
               }
           },
       });

       return <div ref={(node) => ref(drop(node))}>{item}</div>;
   };

   const DragDropContainer = () => {
       const items = useSelector((state) => state.items);

       return (
           <DndProvider backend={HTML5Backend}>
               {items.map((item, index) => (
                   <DraggableItem key={item} item={item} index={index} />
               ))}
           </DndProvider>
       );
   };

   export default DragDropContainer;
   ```

### Vorteile der Verwendung von Redux:
- Zentralisiertes State-Management.
- Einfache Integration mit Middleware wie Redux Thunk oder Saga.
- Debugging durch Redux DevTools.

## Fazit

Das effektive Management des React States und die Synchronisierung von Drag & Drop-Daten mit Redux ermöglichen es, robuste und skalierbare Drag & Drop-Funktionen in React-Anwendungen zu implementieren. Durch den Einsatz von Best Practices und State-Management-Tools wie Redux wird die Wartbarkeit und Erweiterbarkeit der Anwendung verbessert.
