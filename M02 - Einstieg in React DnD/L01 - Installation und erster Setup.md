
# React DnD Schulung: Installation und erster Setup

In dieser Schulung lernen Sie, wie Sie React DnD (Drag and Drop) installieren und konfigurieren, um benutzerfreundliche Drag-and-Drop-Funktionen in React-Anwendungen zu implementieren.

## Installation von React DnD und dem HTML5 Backend

React DnD ist eine leistungsstarke Bibliothek zur Implementierung von Drag-and-Drop-Funktionalität in React-Komponenten. Das HTML5-Backend ermöglicht die Nutzung der nativen Drag-and-Drop-Funktionalität des Browsers.

### Schritt 1: Installation von React DnD und dem HTML5 Backend

1. Installieren Sie React DnD und das HTML5 Backend mit npm oder yarn:
   ```bash
   npm install react-dnd react-dnd-html5-backend
   ```

   Oder mit yarn:
   ```bash
   yarn add react-dnd react-dnd-html5-backend
   ```

2. Überprüfen Sie, dass die Pakete erfolgreich installiert wurden, indem Sie in Ihrer `package.json` nach folgenden Einträgen suchen:
   ```json
   "react-dnd": "^VERSION",
   "react-dnd-html5-backend": "^VERSION"
   ```

## Konfiguration des DnDProvider

Der `DnDProvider` stellt die Drag-and-Drop-Kontextumgebung bereit und ist notwendig, um React DnD in Ihrer Anwendung zu verwenden.

### Schritt 2: Einrichten des DnDProviders

1. Importieren Sie den `DnDProvider` und das `HTML5Backend` in Ihre Hauptdatei (z. B. `App.js` oder `index.js`):
   ```javascript
   import { DndProvider } from 'react-dnd';
   import { HTML5Backend } from 'react-dnd-html5-backend';
   ```

2. Umgeben Sie Ihre Anwendung mit dem `DnDProvider`:
   ```javascript
   function App() {
       return (
           <DndProvider backend={HTML5Backend}>
               <div className="App">
                   <h1>React DnD Beispiel</h1>
                   {/* Ihre Komponenten */}
               </div>
           </DndProvider>
       );
   }

   export default App;
   ```

3. Stellen Sie sicher, dass Ihre Anwendung nun den Kontext für Drag-and-Drop-Funktionen unterstützt.

### Beispiel: Einfaches Setup mit React DnD

Erstellen Sie eine einfache Datei- und Ordnerstruktur für das Projekt:
```plaintext
src/
├── components/
│   ├── DraggableItem.js
│   └── DropTarget.js
└── App.js
```

#### `DraggableItem.js`
Eine Komponente, die als Drag-Element fungiert:
```javascript
import React from 'react';
import { useDrag } from 'react-dnd';

const DraggableItem = ({ name }) => {
    const [{ isDragging }, drag] = useDrag(() => ({
        type: 'ITEM',
        item: { name },
        collect: (monitor) => ({
            isDragging: monitor.isDragging(),
        }),
    }));

    return (
        <div ref={drag} style={{ opacity: isDragging ? 0.5 : 1, cursor: 'move' }}>
            {name}
        </div>
    );
};

export default DraggableItem;
```

#### `DropTarget.js`
Eine Komponente, die als Drop-Ziel fungiert:
```javascript
import React from 'react';
import { useDrop } from 'react-dnd';

const DropTarget = ({ onDrop }) => {
    const [{ isOver }, drop] = useDrop(() => ({
        accept: 'ITEM',
        drop: (item) => onDrop(item),
        collect: (monitor) => ({
            isOver: monitor.isOver(),
        }),
    }));

    return (
        <div ref={drop} style={{ border: '2px dashed gray', padding: '20px', background: isOver ? '#f0f0f0' : 'white' }}>
            {isOver ? 'Lassen Sie los!' : 'Ziehen Sie ein Element hierher'}
        </div>
    );
};

export default DropTarget;
```

#### Aktualisieren von `App.js`
Verknüpfen Sie die Komponenten und testen Sie die Drag-and-Drop-Funktionalität:
```javascript
import React from 'react';
import { DndProvider } from 'react-dnd';
import { HTML5Backend } from 'react-dnd-html5-backend';
import DraggableItem from './components/DraggableItem';
import DropTarget from './components/DropTarget';

function App() {
    const handleDrop = (item) => {
        alert(`Sie haben ${item.name} fallen gelassen!`);
    };

    return (
        <DndProvider backend={HTML5Backend}>
            <div className="App">
                <h1>React DnD Schulung</h1>
                <DraggableItem name="Element 1" />
                <DraggableItem name="Element 2" />
                <DropTarget onDrop={handleDrop} />
            </div>
        </DndProvider>
    );
}

export default App;
```

## Fazit

React DnD und das HTML5 Backend bieten eine einfache Möglichkeit, Drag-and-Drop-Funktionalitäten in React-Anwendungen zu integrieren. Mit dem `DnDProvider` und den Hooks `useDrag` und `useDrop` können Sie leistungsstarke und interaktive Benutzeroberflächen erstellen.
