
# React DnD Schulung: useDrop – Drop-Zonen definieren

In dieser Schulung lernen Sie, wie Sie mit der React DnD-Bibliothek (React Drag and Drop) Drop-Zonen erstellen und konfigurieren. Insbesondere wird der Fokus auf den Hook `useDrop` gelegt, um Drop-Zonen einzurichten und spezifische Anforderungen wie Item-Typen zu behandeln.

## Drop-Zonen mit `useDrop` einrichten

`useDrop` ist ein Hook aus der React DnD-Bibliothek, der es ermöglicht, Komponenten als Ziel für Drag-and-Drop-Operationen zu definieren.

### Installation der React DnD-Bibliothek

Falls React DnD noch nicht in Ihrem Projekt installiert ist, führen Sie die folgenden Befehle aus:
```bash
npm install react-dnd react-dnd-html5-backend
```

### Grundlegendes Beispiel

Ein einfacher Aufbau einer Drop-Zone mit `useDrop`:
```javascript
import React from 'react';
import { useDrop } from 'react-dnd';

const DropZone = () => {
    const [{ isOver }, drop] = useDrop(() => ({
        accept: 'ITEM_TYPE', // Der Typ des Items, das akzeptiert wird
        drop: (item, monitor) => {
            console.log(`Item gedroppt:`, item);
        },
        collect: (monitor) => ({
            isOver: !!monitor.isOver(),
        }),
    }));

    return (
        <div
            ref={drop}
            style={{
                width: '200px',
                height: '200px',
                backgroundColor: isOver ? 'lightgreen' : 'lightgrey',
                border: '2px dashed black',
            }}
        >
            Drop here
        </div>
    );
};

export default DropZone;
```

### Erklärung:
- **`accept`**: Gibt an, welche Typen von Items akzeptiert werden. Dies wird mit `ItemTypes` definiert (siehe unten).
- **`drop`**: Eine Funktion, die aufgerufen wird, wenn ein gültiges Item in der Drop-Zone losgelassen wird.
- **`collect`**: Eine Funktion, die den aktuellen Zustand des Drop-Ziels sammelt, z. B. ob ein Item über der Zone schwebt.

## Bestimmen, welche Items wo gedroppt werden können (ItemTypes)

Mit `ItemTypes` können Sie spezifische Typen definieren, um zu kontrollieren, welche Items in welchen Drop-Zonen akzeptiert werden.

### Beispiel: Definieren von Item-Typen
Erstellen Sie eine Datei `ItemTypes.js`:
```javascript
export const ItemTypes = {
    BOX: 'box',
    CIRCLE: 'circle',
};
```

Verwenden Sie diese Typen in der Drop-Zone:
```javascript
import React from 'react';
import { useDrop } from 'react-dnd';
import { ItemTypes } from './ItemTypes';

const CircleDropZone = () => {
    const [{ isOver }, drop] = useDrop(() => ({
        accept: ItemTypes.CIRCLE, // Akzeptiert nur Items vom Typ "CIRCLE"
        drop: (item) => console.log(`Circle gedroppt:`, item),
        collect: (monitor) => ({
            isOver: !!monitor.isOver(),
        }),
    }));

    return (
        <div
            ref={drop}
            style={{
                width: '200px',
                height: '200px',
                backgroundColor: isOver ? 'lightblue' : 'lightgrey',
                border: '2px dashed blue',
            }}
        >
            Drop Circle Here
        </div>
    );
};

export default CircleDropZone;
```

### Draggable Items erstellen
Verwenden Sie `useDrag`, um Items zu erstellen, die in den Drop-Zonen abgelegt werden können:
```javascript
import React from 'react';
import { useDrag } from 'react-dnd';
import { ItemTypes } from './ItemTypes';

const DraggableCircle = () => {
    const [{ isDragging }, drag] = useDrag(() => ({
        type: ItemTypes.CIRCLE,
        item: { id: 1 },
        collect: (monitor) => ({
            isDragging: !!monitor.isDragging(),
        }),
    }));

    return (
        <div
            ref={drag}
            style={{
                width: '50px',
                height: '50px',
                borderRadius: '50%',
                backgroundColor: 'blue',
                opacity: isDragging ? 0.5 : 1,
                cursor: 'move',
            }}
        ></div>
    );
};

export default DraggableCircle;
```

### Integration
Kombinieren Sie die Drop-Zonen und draggable Items:
```javascript
import React from 'react';
import CircleDropZone from './CircleDropZone';
import DraggableCircle from './DraggableCircle';

const App = () => {
    return (
        <div style={{ display: 'flex', gap: '20px', padding: '20px' }}>
            <DraggableCircle />
            <CircleDropZone />
        </div>
    );
};

export default App;
```

## Fazit

Mit `useDrop` können Sie leistungsstarke und flexible Drop-Zonen in React-Anwendungen erstellen. Durch die Verwendung von `ItemTypes` lässt sich präzise steuern, welche Items in welche Drop-Zonen passen. In Kombination mit `useDrag` bietet React DnD eine robuste Lösung für Drag-and-Drop-Interaktionen.
