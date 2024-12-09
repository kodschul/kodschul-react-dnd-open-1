
# React DnD Schulung: Erste Schritte mit `useDrag`

In dieser Schulung lernen Sie die Grundlagen von React DnD (Drag and Drop) kennen, insbesondere die Verwendung des `useDrag`-Hooks, um Elemente draggable zu machen und Zustandsdaten während eines Drag-and-Drop-Vorgangs zu sammeln.

## Erste Schritte mit `useDrag`

Der Hook `useDrag` ist das Kernstück von React DnD, das es ermöglicht, ein Element als "draggable" zu markieren. Mit `useDrag` können Sie:
- Definieren, welche Daten während des Drag-Vorgangs übertragen werden.
- Drag-Ereignisse verfolgen und Zustandsdaten verwalten.

### Voraussetzungen
1. **Installation von React DnD**:
   Installieren Sie die Bibliothek und das gewünschte Backend:
   ```bash
   npm install react-dnd react-dnd-html5-backend
   ```

2. **Setup des DnD-Providers**:
   Um Drag-and-Drop-Funktionalität zu aktivieren, muss der `DndProvider` in Ihrem Projekt um Ihre App-Komponenten gewickelt werden:
   ```jsx
   import { DndProvider } from 'react-dnd';
   import { HTML5Backend } from 'react-dnd-html5-backend';

   function App() {
       return (
           <DndProvider backend={HTML5Backend}>
               <YourComponent />
           </DndProvider>
       );
   }
   ```

## Ein Element draggable machen mit `useDrag`

Mit `useDrag` können Sie ein Element draggable machen. Der Hook gibt ein Array zurück, das ein `dragRef` (Ref für das DOM-Element) und Dragging-Zustandsdaten enthält.

### Grundlegende Verwendung:
```jsx
import { useDrag } from 'react-dnd';

function DraggableItem() {
    const [{ isDragging }, dragRef] = useDrag(() => ({
        type: 'BOX', // Typ des draggables, muss mit `useDrop` übereinstimmen
        item: { id: 1 }, // Daten, die beim Drag übertragen werden
        collect: (monitor) => ({
            isDragging: monitor.isDragging(), // Zustand des Draggings
        }),
    }));

    return (
        <div
            ref={dragRef}
            style={{
                opacity: isDragging ? 0.5 : 1, // Reduziert die Sichtbarkeit während des Drag-Vorgangs
                cursor: 'move',
            }}
        >
            Drag mich!
        </div>
    );
}
```

### Erklärungen:
- **`type`**: Ein eindeutiger Typ, der das draggable Element identifiziert. Es sollte mit einem Drop-Target übereinstimmen.
- **`item`**: Daten, die an das Drop-Target übergeben werden.
- **`collect`**: Eine Funktion, die Daten und Zustände vom Drag-Monitor sammelt.

## Collector-Funktionen: Daten und Zustand eines Drags sammeln

Collector-Funktionen sind ein wesentlicher Bestandteil von `useDrag`. Sie ermöglichen den Zugriff auf Drag-Zustandsdaten, z. B. ob ein Element gerade gezogen wird.

### Beispiel einer erweiterten Collector-Funktion:
```jsx
function DraggableCard({ id, text }) {
    const [{ isDragging, initialOffset, currentOffset }, dragRef] = useDrag(() => ({
        type: 'CARD',
        item: { id },
        collect: (monitor) => ({
            isDragging: monitor.isDragging(),
            initialOffset: monitor.getInitialSourceClientOffset(),
            currentOffset: monitor.getSourceClientOffset(),
        }),
    }));

    return (
        <div
            ref={dragRef}
            style={{
                opacity: isDragging ? 0.5 : 1,
                transform: currentOffset
                    ? `translate(${currentOffset.x}px, ${currentOffset.y}px)`
                    : undefined,
                cursor: 'move',
            }}
        >
            {text}
        </div>
    );
}
```

### Verfügbare Methoden im `monitor`-Objekt:
- **`isDragging()`**: Gibt zurück, ob das aktuelle Element gerade gezogen wird.
- **`getItem()`**: Gibt die übertragenen Daten des Items zurück.
- **`getInitialSourceClientOffset()`**: Gibt die initiale Position des Elements zurück.
- **`getSourceClientOffset()`**: Gibt die aktuelle Position des Elements zurück.

### Tipps für die Verwendung:
- Verwenden Sie `isDragging`, um visuelles Feedback (z. B. Transparenz) zu implementieren.
- Nutzen Sie `currentOffset`, um die Bewegung eines Elements dynamisch zu verfolgen.

## Fazit

Mit dem `useDrag`-Hook von React DnD können Sie leistungsstarke Drag-and-Drop-Interaktionen in Ihren React-Anwendungen erstellen. Durch die Kombination von Typen, Item-Daten und Collector-Funktionen haben Sie die volle Kontrolle über die Logik und das visuelle Feedback während eines Drag-and-Drop-Vorgangs.
