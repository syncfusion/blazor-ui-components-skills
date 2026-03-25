# Interaction in Blazor Diagram

## Table of Contents
- [Overview](#overview)
- [Selection](#selection)
- [Drag and Drop](#drag-and-drop)
- [Resize and Rotate](#resize-and-rotate)
- [Zoom and Pan](#zoom-and-pan)
- [BringIntoView and BringIntoCenter](#bringintoview-and-bringintocenter)
- [FitToPage Command](#fittopage-command)
- [Nudge Command](#nudge-command)
- [Z-Order Commands](#z-order-commands)
- [Keyboard Shortcuts](#keyboard-shortcuts)
- [Command Manager (Custom Keyboard Shortcuts)](#command-manager-custom-keyboard-shortcuts)
- [Snapping](#snapping)
- [Alignment, Spacing, and Sizing Commands](#alignment-spacing-and-sizing-commands)
- [Undo and Redo](#undo-and-redo)
- [User Handles](#user-handles)
- [Drawing Tool](#drawing-tool)
- [Common Gotchas](#common-gotchas)

---

## Overview

The Blazor Diagram provides rich user interactions: select, drag, resize, rotate, zoom, pan, keyboard shortcuts, snapping, undo/redo, and more.

---

## Selection

**Single selection:** Click any node or connector.

**Multi-selection:** Ctrl+Click or rubber-band drag on empty area.

**Programmatic selection:**
```csharp
// Select a node
_diagram.Select(new ObservableCollection<IDiagramObject> { _diagram.Nodes[0] });

// Clear selection
_diagram.ClearSelection();
```

**Selection events:**
```razor
<SfDiagramComponent SelectionChanging="OnSelectionChanging"
                    SelectionChanged="OnSelectionChanged" />

@code {
    private void OnSelectionChanging(SelectionChangingEventArgs args)
    {
        args.Cancel = true; // prevent selection
    }

    private void OnSelectionChanged(SelectionChangedEventArgs args)
    {
        // args.NewValue — newly selected elements
        // args.OldValue — previously selected elements
    }
}
```

---

## Drag and Drop

Users drag nodes/connectors by default. Disable per-element:
```razor
new Node
{
    Constraints = NodeConstraints.Default & ~NodeConstraints.Drag
}
```

**Drag events:**
```razor
<SfDiagramComponent PositionChanging="OnPositionChanging"
                    PositionChanged="OnPositionChanged" />

@code {
    private void OnPositionChanging(PositionChangingEventArgs args)
    {
        args.Cancel = true; // prevent the move
    }

    private void OnPositionChanged(PositionChangedEventArgs args)
    {
        // args.NewValue.OffsetX / OffsetY — new position
    }
}
```

---

## Resize and Rotate

Users can resize (drag handles) and rotate (rotation handle) selected nodes.

**Disable resize or rotate per node:**
```razor
new Node
{
    Constraints = NodeConstraints.Default
        & ~NodeConstraints.Resize
        & ~NodeConstraints.Rotate
}
```

**Size change events:**
```razor
<SfDiagramComponent SizeChanging="OnSizeChanging" SizeChanged="OnSizeChanged" />
```

---

## Zoom and Pan

**Mouse/trackpad:** Ctrl+Scroll to zoom, scroll/middle-drag to pan.

**Programmatic zoom:**
```csharp
// Zoom in to a point
_diagram.Zoom(1.2, new DiagramPoint { X = 300, Y = 300 });

// Fit entire diagram in view
_diagram.FitToPage();

// Reset zoom
_diagram.ResetZoom();
```

**Disable pan via toolbar (use FitToPage instead):**
```razor
<SfDiagramComponent InteractionController="@DiagramInteractions.ZoomPan" />
// or
<SfDiagramComponent InteractionController="@DiagramInteractions.Default" />
```

---

## BringIntoView and BringIntoCenter

Scroll the viewport to make a specific rectangular region visible or centered, without changing the zoom level.

**BringIntoView** — adjusts the scroll position so the region is visible within the viewport:
```csharp
// DiagramRect(double x, double y, double width, double height)
DiagramRect bound = new DiagramRect(950, 650, 500, 500);
_diagram.BringIntoView(bound);
```

**BringIntoCenter** — scrolls and centers the viewport on the specified region:
```csharp
DiagramRect bound = new DiagramRect(950, 650, 500, 500);
_diagram.BringIntoCenter(bound);
```

> **Tip:** Use `node.OffsetX - node.Width / 2` and `node.OffsetY - node.Height / 2` to compute the `DiagramRect` from a node's position and size.

---

## FitToPage Command

Adjusts the zoom level and scroll position so that all diagram content (or a specified region) fits within the viewport.

```csharp
// Default — fits all content with default options
_diagram.FitToPage();

// With FitOptions
var options = new FitOptions()
{
    Mode = FitMode.Both,                  // FitMode.Width, FitMode.Height, or FitMode.Both
    Region = DiagramRegion.Content,       // DiagramRegion.Content or DiagramRegion.PageSettings
    CanZoomIn = false                     // true allows zooming IN for small content; default false
};
_diagram.FitToPage(options);
```

**FitMode values:**
| Value | Behavior |
|-------|----------|
| `FitMode.Width` | Fits diagram width to viewport |
| `FitMode.Height` | Fits diagram height to viewport |
| `FitMode.Both` | Fits both dimensions (default) |

**DiagramRegion values:**
| Value | Behavior |
|-------|----------|
| `DiagramRegion.Content` | Fits to the bounding box of all content |
| `DiagramRegion.PageSettings` | Fits to the configured page size |

> ⚠️ `FitMode.Page` does **NOT** exist — see Common Mistakes in SKILL.md.  
> ⚠️ `FitToPageAsync()` does **NOT** exist — `FitToPage()` is synchronous.

---

## Nudge Command

Moves selected elements in a specified direction by a given number of pixels.

```csharp
// Move selected elements 50px to the left
_diagram.Nudge(Direction.Left, 50);

// Move 10px to the right
_diagram.Nudge(Direction.Right, 10);

// Move 1px up (default delta when omitted)
_diagram.Nudge(Direction.Top);

// Move 1px down
_diagram.Nudge(Direction.Bottom);
```

**Direction enum values:**
| Value | Movement |
|-------|----------|
| `Direction.Left` | Move left |
| `Direction.Right` | Move right |
| `Direction.Top` | Move up |
| `Direction.Bottom` | Move down |

> **Note:** Arrow keys also nudge selected elements by 1px by default when the diagram has keyboard focus.  
> **Tip:** Call `_diagram.Select(...)` first to programmatically set the selection before nudging.

---

## Z-Order Commands

Control which elements appear in front of or behind others. **You must select the target element first.**

```csharp
// Select the node to reorder
_diagram.Select(new ObservableCollection<IDiagramObject>() { _diagram.Nodes[0] });

// Move to the front (top of z-stack — drawn last, appears on top)
_diagram.BringToFront();

// Move one step forward (increase z-order by 1)
_diagram.BringForward();

// Move one step backward (decrease z-order by 1)
_diagram.SendBackward();

// Move to the back (bottom of z-stack — drawn first, appears behind all others)
_diagram.SendToBack();
```

> ⚠️ All z-order methods are **synchronous** and operate on the **currently selected** elements.  
> Always call `_diagram.Select(...)` before calling a z-order method.

---

## Command Manager (Custom Keyboard Shortcuts)

Use `CommandManager` to define custom keyboard shortcuts or override built-in ones. It is a **child component** nested inside `SfDiagramComponent`.

**Razor markup:**
```razor
<SfDiagramComponent @ref="@_diagram" Height="600px" Nodes="@_nodes">
    <CommandManager Commands="@_command" Execute="@CommandExecute" CanExecute="@CanExe">
    </CommandManager>
</SfDiagramComponent>
```

**Code-behind:**
```csharp
private DiagramObjectCollection<KeyboardCommand> _command = new()
{
    new KeyboardCommand()
    {
        Name = "CustomGroup",
        Gesture = new KeyGesture() { Key = DiagramKeys.G, Modifiers = ModifierKeys.Control }
    },
    new KeyboardCommand()
    {
        Name = "CustomUngroup",
        Gesture = new KeyGesture() { Key = DiagramKeys.U, Modifiers = ModifierKeys.Control }
    }
};

// Must set args.CanExecute = true to allow the command to fire
private void CanExe(CommandKeyArgs args)
{
    args.CanExecute = true;
}

// Perform the action when the shortcut is triggered
private void CommandExecute(CommandKeyArgs args)
{
    if (args.Gesture.Modifiers == ModifierKeys.Control && args.Gesture.Key == DiagramKeys.G)
        _diagram.Group();

    if (args.Gesture.Modifiers == ModifierKeys.Control && args.Gesture.Key == DiagramKeys.U)
    {
        var selector = _diagram.SelectionSettings;
        if (selector.Nodes.Count > 0 && selector.Nodes[0] is NodeGroup ng && ng.Children.Length > 0)
            _diagram.Ungroup();
    }
}
```

**Key types:**
- `DiagramKeys` — keyboard key values (e.g., `DiagramKeys.G`, `DiagramKeys.Delete`, `DiagramKeys.F2`)
- `ModifierKeys` — modifier keys (e.g., `ModifierKeys.Control`, `ModifierKeys.Shift`, `ModifierKeys.Alt`)
- `KeyGesture` — combines a `Key` and optional `Modifiers`
- `CommandKeyArgs` — event args for both `Execute` and `CanExecute` callbacks

**Override or disable built-in shortcuts:**
```csharp
// Add a command with the same gesture as a built-in shortcut
new KeyboardCommand()
{
    Name = "DisableDelete",
    Gesture = new KeyGesture() { Key = DiagramKeys.Delete }
}

// In Execute — do nothing to effectively disable it
private void CommandExecute(CommandKeyArgs args)
{
    if (args.Gesture.Key == DiagramKeys.Delete)
        return; // built-in delete is suppressed
}
```

---

## Keyboard Shortcuts

Built-in keyboard shortcuts (active when diagram has focus):

| Shortcut | Action |
|----------|--------|
| Ctrl+Z | Undo |
| Ctrl+Y | Redo |
| Ctrl+C | Copy |
| Ctrl+V | Paste |
| Ctrl+X | Cut |
| Delete | Delete selected |
| Ctrl+A | Select all |
| Arrow keys | Move selected by 1px |
| Shift+Arrow | Move selected by 5px |
| Ctrl+Shift+B | Send to back |
| Ctrl+Shift+F | Bring to front |

**Custom keyboard shortcuts:**
```razor
<SfDiagramComponent KeyDown="OnKeyDown" />

@code {
    private void OnKeyDown(KeyEventArgs args)
    {
        if (args.Key == "Delete")
        {
            // custom delete logic
        }
    }
}
```

---

## Snapping

Snap nodes to grid or to other elements for precise alignment:

```razor
<SfDiagramComponent>
    <SnapSettings Constraints="SnapConstraints.ShowLines | SnapConstraints.SnapToLines">
        <HorizontalGridLines LineColor="#e0e0e0" LineDashArray="2,2" LineIntervals="@lineIntervals" />
        <VerticalGridLines LineColor="#e0e0e0" LineDashArray="2,2" LineIntervals="@lineIntervals" />
    </SnapSettings>
</SfDiagramComponent>

@code {
    double[] lineIntervals = { 1, 9, 0.25, 9.75, 0.25, 9.75, 0.25, 9.75, 0.25, 9.75 };
}
```

**Snap to objects only (no grid):**
```razor
<SnapSettings Constraints="SnapConstraints.SnapToObject" />
```

**Disable snapping:**
```razor
<SnapSettings Constraints="SnapConstraints.None" />
```

**Object snap distance (how close before snapping kicks in):**
```razor
<SnapSettings Constraints="SnapConstraints.SnapToObject" SnapDistance="5" />
```

> **⚠️ `SnapObjectDistance` does NOT exist** on `SnapSettings` — using it causes `InvalidOperationException: does not have a property matching the name 'SnapObjectDistance'`.  
> The correct property name is **`SnapDistance`**:
> ```razor
> @* ❌ Wrong — SnapObjectDistance does not exist *@
> <SnapSettings SnapObjectDistance="5" />
>
> @* ✅ Correct *@
> <SnapSettings SnapDistance="5" />
> ```

---

## Alignment, Spacing, and Sizing Commands

Programmatic commands to organize selected elements:

```csharp
// Align selected nodes
_diagram.SetAlign(AlignmentOptions.Left);   // align left
_diagram.SetAlign(AlignmentOptions.Right);  // align right
_diagram.SetAlign(AlignmentOptions.Top);    // align top
_diagram.SetAlign(AlignmentOptions.Bottom); // align bottom
_diagram.SetAlign(AlignmentOptions.Center); // align center
_diagram.SetAlign(AlignmentOptions.Middle); // align middle

// Distribute spacing evenly
_diagram.SetDistribute(DistributeOptions.Left);         // distribute left
_diagram.SetDistribute(DistributeOptions.Right);        // distribute right
_diagram.SetDistribute(DistributeOptions.Top);          // distribute top
_diagram.SetDistribute(DistributeOptions.Bottom);       // distribute bottom
_diagram.SetDistribute(DistributeOptions.Middle);       // distribute middle
_diagram.SetDistribute(DistributeOptions.Center);       // distribute center
_diagram.SetDistribute(DistributeOptions.BottomToTop);  // distribute bottomToTop
_diagram.SetDistribute(DistributeOptions.RightToLeft);  // distribute rightToLeft

// Same size
_diagram.SetSameSize(SizingMode.Width);   // same width as first selected
_diagram.SetSameSize(SizingMode.Height);  // same height
_diagram.SetSameSize(SizingMode.Both);    // same width AND height
```

---

## Undo and Redo

```csharp
// Undo last action
_diagram.Undo();

// Redo
_diagram.Redo();

// Check state
bool canUndo = _diagram.HistoryManager.CanUndo;
bool canRedo = _diagram.HistoryManager.CanRedo;
```

**History change event:**
```razor
<SfDiagramComponent HistoryChanged="OnHistoryChanged" />

@code {
    private void OnHistoryChanged(HistoryChangedEventArgs args)
    {
        // args.Action — the history action taken
    }
}
```

---

## User Handles

Custom action buttons that appear on selected elements:

```razor
@using System.Collections.ObjectModel
@using Syncfusion.Blazor.Diagram

<SfDiagramComponent @ref="_diagram" Height="500px"
                    Nodes="@_nodes"
                    Connectors="@_connectors"
                    SelectionSettings="@_selectedModel"
                    GetCustomTool="GetCustomTool">
</SfDiagramComponent>

@code
{
    private SfDiagramComponent _diagram;
    private DiagramObjectCollection<Node> _nodes { get; set; }
    private DiagramObjectCollection<Connector> _connectors { get; set; }
    // Defines diagram's SelectionSettings.
    private DiagramSelectionSettings _selectedModel = new DiagramSelectionSettings();
    private DiagramObjectCollection<UserHandle> _userHandles = new DiagramObjectCollection<UserHandle>();

    protected override void OnInitialized()
    {
        _nodes = new DiagramObjectCollection<Node>();
        _connectors = new DiagramObjectCollection<Connector>();
        InitDiagramModel();
    }

    private InteractionControllerBase GetCustomTool(DiagramElementAction action, string id)
    {
        return id == "clone" ? new CloneTool(_diagram) : new DeleteTool(_diagram);
    }

    private class DeleteTool : InteractionControllerBase
    {
        private SfDiagramComponent _diagram;
        public DeleteTool(SfDiagramComponent diagram) : base(diagram) { _diagram = diagram; }
        public override void OnMouseUp(DiagramMouseEventArgs args)
        {
            _diagram.Delete();
            base.OnMouseUp(args);
        }
    }

    private class CloneTool : DragController
    {
        private SfDiagramComponent _diagram;
        public CloneTool(SfDiagramComponent diagram) : base(diagram) { _diagram = diagram; }
        public override void OnMouseDown(DiagramMouseEventArgs args)
        {
            _diagram.Copy();
            _diagram.Paste();
            base.OnMouseDown(args);
        }
    }

    private void InitDiagramModel()
    {
        Node node = new Node()
        {
            ID = "Node1",
            OffsetX = 300,
            OffsetY = 200,
            Width = 100,
            Height = 100,
            Style = new ShapeStyle() { Fill = "#6495ED", StrokeColor = "none" },
            Annotations = new DiagramObjectCollection<ShapeAnnotation>()
            {
                new ShapeAnnotation() { Content = "Node" }
            }
        };
        _nodes.Add(node);

        Connector connector = new Connector()
        {
            ID = "Connector1",
            SourcePoint = new DiagramPoint() { X = 500, Y = 150 },
            TargetPoint = new DiagramPoint() { X = 600, Y = 250 },
            Type = ConnectorSegmentType.Orthogonal,
            Annotations = new DiagramObjectCollection<PathAnnotation>()
            {
                new PathAnnotation(){ Content = "Connector" }
            }
        };
        _connectors.Add(connector);
        UserHandle cloneHandle = new UserHandle()
        {
            Name = "clone",
            PathData = "M60.3,18H27.5c-3,0-5.5,2.4-5.5,5.5v38.2h5.5V23.5h32.7V18z M68.5,28.9h-30c-3,0-5.5,2.4-5.5,5.5v38.2c0,3,2.4,5.5,5.5,5.5h30c3,0,5.5-2.4,5.5-5.5V34.4C73.9,31.4,71.5,28.9,68.5,28.9z M68.5,72.5h-30V34.4h30V72.5z",
            Offset = 0,
            Side = Direction.Right,
            Visible = true,
            VisibleTarget = VisibleTarget.Node | VisibleTarget.Connector
        };
        UserHandle deleteHandle = new UserHandle()
        {
            Name = "delete",
            PathData = "M0.54700077,2.2130003 L7.2129992,2.2130003 7.2129992,8.8800011 C7.2129992,9.1920013 7.1049975,9.4570007 6.8879985,9.6739998 6.6709994,9.8910007 6.406,10 6.0939997,10 L1.6659999,10 C1.3539997,10 1.0890004,9.8910007 0.87200136,9.6739998 0.65500242,9.4570007 0.54700071,9.1920013 0.54700077,8.8800011 z M2.4999992,0 L5.2600006,0 5.8329986,0.54600048 7.7599996,0.54600048 7.7599996,1.6660004 0,1.6660004 0,0.54600048 1.9270014,0.54600048 z",
            Offset = 1,
            Side = Direction.Bottom,
            VisibleTarget = VisibleTarget.Node | VisibleTarget.Connector,
            Visible = true
        };
        _userHandles = new DiagramObjectCollection<UserHandle>()
        {
            cloneHandle, deleteHandle
        };
        _selectedModel = new DiagramSelectionSettings()
        {
            //Enable userhandle for selected model.
            Constraints = SelectorConstraints.UserHandle,

            UserHandles = _userHandles
        };
    }
}
```

---

## Drawing Tool

Let users draw nodes or connectors by clicking and dragging:

```razor
<SfDiagramComponent @ref="_diagram"
                    InteractionController="DiagramInteractions.DrawOnce"
                    DrawingObject="@drawingObj" />

@code {
    IDiagramObject drawingObj = new Node
    {
        Shape = new FlowShape { Type = NodeShapes.Flow, Shape = NodeFlowShapes.Process }
    };

    // Switch to draw connector mode:
    private void DrawConnector()
    {
        _diagram.InteractionController = DiagramInteractions.DrawOnce;
        _diagram.DrawingObject = new Connector { Type = ConnectorSegmentType.Orthogonal };
    }
}
```

---

## Common Gotchas

- **Selection is enabled by default** — use `NodeConstraints` to restrict it per node
- **Rubber-band selection** requires clicking and dragging on the empty canvas (not on a node)
- **`FitToPage()`** must be called after the diagram renders — use `OnAfterRenderAsync` or the `Created` event; it is **synchronous** (no `await`)
- **Z-order methods** (`BringToFront`, `SendToBack`, etc.) require the target to be **selected first** via `_diagram.Select(...)`
- **`CommandManager`** must be a **child component** of `SfDiagramComponent`, not a sibling — `CanExecute` must set `args.CanExecute = true` or the command won't fire
- **Undo/redo tracks all state changes** — use `BeginUpdate()`/`EndUpdateAsync()` to group changes into a single undo step
- **User handles appear only when an element is selected** — set `Visible = true` to show them by default
- **`InteractionController = DiagramInteractions.DrawOnce`** switches to draw mode for one element, then reverts; use `ContinuesDraw` to keep drawing
- **`SnapObjectDistance` does NOT exist** on `SnapSettings` — causes `InvalidOperationException`. Use `SnapDistance` instead: `<SnapSettings SnapDistance="5" />`
