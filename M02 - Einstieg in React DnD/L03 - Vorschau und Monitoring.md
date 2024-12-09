
# React DnD Schulung: Vorschau und Monitoring

In dieser Schulung lernen Sie, wie Sie mit React DnD (Drag and Drop) DragPreviewImage zur Vorschau von Dragging-Elementen verwenden und CSS-Styles während des Drag-Vorgangs dynamisch anpassen können.

## DragPreviewImage: Vorschau für Dragging-Elemente erstellen

React DnD bietet die Möglichkeit, benutzerdefinierte Vorschauen für Elemente zu erstellen, die während des Dragging angezeigt werden. Dies geschieht mit der Komponente `DragPreviewImage`.

### Vorteile von DragPreviewImage:
- Verbesserte Benutzererfahrung durch anpassbare Vorschau-Elemente.
- Kontrolle über die Darstellung des Dragging-Elements.

### Beispiel: Verwendung von DragPreviewImage
```jsx
import React from 'react';
import { useDrag, DragPreviewImage } from 'react-dnd';

const DraggableItem = () => {
    const [{ isDragging }, drag, preview] = useDrag(() => ({
        type: 'ITEM',
        collect: (monitor) => ({
            isDragging: monitor.isDragging(),
        }),
    }));

    return (
        <>
            {/* Vorschau-Image */}
            <DragPreviewImage connect={preview} src="/path-to-preview-image.png" />
            {/* Drag-Element */}
            <div
                ref={drag}
                style={{
                    opacity: isDragging ? 0.5 : 1,
                    cursor: 'move',
                }}
            >
                Drag mich!
            </div>
        </>
    );
};

export default DraggableItem;
```

### Schritte:
1. **Verwenden von `useDrag`**:
   - `useDrag` wird verwendet, um das Dragging-Element zu registrieren und seine Eigenschaften zu überwachen.
2. **Einfügen von DragPreviewImage**:
   - `DragPreviewImage` verbindet sich mit dem Preview-Ref (`preview`) und zeigt das angegebene Bild während des Dragging an.

## CSS-Manipulation während des Drags

Während des Dragging können Sie das Aussehen der Elemente durch dynamische Änderungen der CSS-Stile anpassen.

### Beispiel: Anpassung der Stile während des Drags
```jsx
import React from 'react';
import { useDrag } from 'react-dnd';

const DraggableBox = () => {
    const [{ isDragging }, drag] = useDrag(() => ({
        type: 'BOX',
        collect: (monitor) => ({
            isDragging: monitor.isDragging(),
        }),
    }));

    return (
        <div
            ref={drag}
            style={{
                width: '100px',
                height: '100px',
                backgroundColor: isDragging ? 'lightblue' : 'steelblue',
                opacity: isDragging ? 0.6 : 1,
                transform: isDragging ? 'scale(1.1)' : 'none',
                cursor: 'move',
            }}
        >
            Box
        </div>
    );
};

export default DraggableBox;
```

### Schritte:
1. **Überwachen des Dragging-Status**:
   - Der Status `isDragging` wird über die `collect`-Funktion von `useDrag` erfasst.
2. **CSS-Stile dynamisch anpassen**:
   - Ändern Sie die Eigenschaften wie `backgroundColor`, `opacity` oder `transform` basierend auf dem Status `isDragging`.

### Tipps für CSS-Manipulation:
- Nutzen Sie `transition`-Eigenschaften, um fließende Animationen während des Dragging zu erzeugen.
- Vermeiden Sie zu viele visuelle Effekte, die die Benutzerfreundlichkeit beeinträchtigen könnten.

## Fazit

Mit React DnD können Sie benutzerdefinierte Dragging-Vorschauen und dynamische CSS-Manipulationen implementieren, um die Interaktion mit Drag-and-Drop-Elementen zu verbessern. Die Kombination von `DragPreviewImage` und dynamischem CSS ermöglicht eine flexible Gestaltung und Anpassung an verschiedene Anforderungen.
