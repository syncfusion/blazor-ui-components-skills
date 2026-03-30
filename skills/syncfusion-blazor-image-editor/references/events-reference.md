# Events Reference for the Image Editor

## Overview

The Image Editor provides comprehensive event support through the `ImageEditorEvents` component. Events are triggered before and after operations, enabling validation, customization, and tracking of user actions.

## Table of Contents
- [Event Structure](#event-structure)
- [Lifecycle Events](#lifecycle-events)
- [File Operations Events](#file-operations-events)
- [Transformation Events](#transformation-events)
- [Shape and Annotation Events](#shape-and-annotation-events)
- [Adjustment Events](#adjustment-events)
- [User Interaction Events](#user-interaction-events)
- [Toolbar Events](#toolbar-events)
- [Event Arguments Reference](#event-arguments-reference)

## Event Structure

### ImageEditorEvents Component

All events are configured within the `ImageEditorEvents` component:

```csharp
@using Syncfusion.Blazor.ImageEditor

<SfImageEditor @ref="ImageEditor" Height="500px">
    <ImageEditorEvents
        Created="OnCreated"
        Destroyed="OnDestroyed"
        FileOpened="OnFileOpened"
        Saving="OnSaving"
        Cropping="OnCropping"
        Cropped="OnCropped"
        Rotating="OnRotating"
        Rotated="OnRotated"
        Flipping="OnFlipping"
        Flipped="OnFlipped"
        ShapeChanging="OnShapeChanging"
        ShapeChanged="OnShapeChanged"
        ImageFiltering="OnImageFiltering"
        ImageFiltered="OnImageFiltered"
        FinetuneValueChanging="OnFinetuneValueChanging"
        FinetuneValueChanged="OnFinetuneValueChanged"
        FrameChanging="OnFrameChanging"
        FrameChanged="OnFrameChanged"
        ImageResizing="OnImageResizing"
        ImageResized="OnImageResized"
        Zooming="OnZooming"
        Zoomed="OnZoomed"
        OnPanStart="OnPanStart"
        OnPanEnd="OnPanEnd"
        OnShapeResizeStart="OnShapeResizeStart"
        OnShapeResizeEnd="OnShapeResizeEnd"
        OnShapeDragStart="OnShapeDragStart"
        OnShapeDragEnd="OnShapeDragEnd"
        OnSelectionResizeStart="OnSelectionResizeStart"
        OnSelectionResizeEnd="OnSelectionResizeEnd"
        ToolbarUpdating="OnToolbarUpdating"
        ToolbarItemClicked="OnToolbarItemClicked"
        QuickAccessToolbarOpening="OnQuickAccessToolbarOpening"
        HistoryChanged="OnHistoryChanged"
        Clicked="OnClicked">
    </ImageEditorEvents>
</SfImageEditor>

@code {
    SfImageEditor ImageEditor;
    
    // Event handler implementations...
}
```

## Lifecycle Events

### Created Event

Triggered after the Image Editor component is initialized and rendered.

```csharp
private async void OnCreated()
{
    Console.WriteLine("Image Editor initialized");
    
    // Load initial image
    await ImageEditor.OpenAsync("YOUR_IMAGE_URL");
    
    // Configure settings
    // Ready to use all component methods
}
```

**Use Cases:**
- Load initial images
- Configure component settings
- Initialize application state
- Set default values

### Destroyed Event

Triggered before the Image Editor component is destroyed.

```csharp
private void OnDestroyed()
{
    Console.WriteLine("Image Editor destroyed");
    
    // Clean up resources
    // Save state if needed
    // Remove event listeners
}
```

**Use Cases:**
- Resource cleanup
- Save user preferences
- Cancel pending operations
- Log component lifecycle

## File Operations Events

### FileOpened Event

Triggered after an image is successfully opened in the editor.

```csharp
private void OnFileOpened(FileOpenEventArgs args)
{
    Console.WriteLine($"File opened: {args.FileName}");
    Console.WriteLine($"File size: {args.FileSize} bytes");
    Console.WriteLine($"File type: {args.FileType}");
    
    // Update UI
    // Log file information
    // Enable/disable features based on file type
}
```

**FileOpenEventArgs Properties:**
- `FileName` (string): Name of the opened file
- `FileSize` (long): File size in bytes
- `FileType` (ImageEditorFileType): File format (PNG, JPEG, etc.)

### Saving Event

Triggered before the image is saved/exported.

```csharp
private void OnSaving(SaveEventArgs args)
{
    Console.WriteLine($"Saving as: {args.FileName}");
    Console.WriteLine($"Format: {args.FileType}");
    Console.WriteLine($"Quality: {args.ImageQuality}");
    
    // Validate file name
    // Check disk space
    // Show progress indicator
    
    // Cancel save if needed
    // args.Cancel = true;
}
```

**SaveEventArgs Properties:**
- `FileName` (string): Target file name
- `FileType` (ImageEditorFileType): Export format
- `ImageQuality` (double): Quality for JPEG (0-1)
- `Cancel` (bool): Set to true to cancel operation

## Transformation Events

### Cropping Event

Triggered before crop operation is applied.

```csharp
private void OnCropping(CropEventArgs args)
{
    Console.WriteLine("Crop operation starting");
    Console.WriteLine($"Crop area: {args.StartX}, {args.StartY}");
    Console.WriteLine($"Dimensions: {args.Width}x{args.Height}");
    
    // Validate crop dimensions
    if (args.Width < 100 || args.Height < 100)
    {
        Console.WriteLine("Crop area too small");
        args.Cancel = true;
    }
}
```

**CropEventArgs Properties:**
- `StartX` (double): X-coordinate of crop region
- `StartY` (double): Y-coordinate of crop region
- `Width` (double): Crop width
- `Height` (double): Crop height
- `Cancel` (bool): Cancel the operation

### Cropped Event

Triggered after crop operation is completed.

```csharp
private void OnCropped(CroppedEventArgs args)
{
    Console.WriteLine("Crop completed");
    Console.WriteLine($"New dimensions: {args.Width}x{args.Height}");
    
    // Update UI
    // Log action
    // Notify user
}
```

**CroppedEventArgs Properties:**
- `Width` (double): Final image width
- `Height` (double): Final image height

### Rotating Event

Triggered before rotation is applied.

```csharp
private void OnRotating(RotateEventArgs args)
{
    Console.WriteLine($"Rotating by {args.Degree} degrees");
    
    // Validate rotation
    // Show loading indicator
    
    // Cancel if needed
    // args.Cancel = true;
}
```

**RotateEventArgs Properties:**
- `Degree` (int): Rotation angle
- `Cancel` (bool): Cancel the operation

### Rotated Event

Triggered after rotation is completed.

```csharp
private void OnRotated(RotatedEventArgs args)
{
    Console.WriteLine($"Rotation completed: {args.Degree} degrees");
    
    // Update UI
    // Enable undo button
}
```

**RotatedEventArgs Properties:**
- `Degree` (int): Applied rotation angle

### Flipping Event

Triggered before flip operation.

```csharp
private void OnFlipping(FlipEventArgs args)
{
    Console.WriteLine($"Flipping: {args.Direction}");
    
    // Validate flip
    // Show indicator
    
    // Cancel if needed
    // args.Cancel = true;
}
```

**FlipEventArgs Properties:**
- `Direction` (ImageEditorDirection): Horizontal or Vertical
- `Cancel` (bool): Cancel the operation

### Flipped Event

Triggered after flip operation is completed.

```csharp
private void OnFlipped(FlippedEventArgs args)
{
    Console.WriteLine($"Flipped: {args.Direction}");
    
    // Update UI
}
```

**FlippedEventArgs Properties:**
- `Direction` (ImageEditorDirection): Applied flip direction

## Shape and Annotation Events

### ShapeChanging Event

Triggered when a shape is being inserted, selected, or modified.

```csharp
private void OnShapeChanging(ShapeChangeEventArgs args)
{
    Console.WriteLine($"Action: {args.Action}");
    Console.WriteLine($"Shape Type: {args.CurrentShapeSettings.Type}");
    
    // Customize shape appearance
    if (args.Action == "insert")
    {
        if (args.CurrentShapeSettings.Type == ShapeType.Text)
        {
            args.CurrentShapeSettings.Color = "blue";
            args.CurrentShapeSettings.FontSize = 24;
        }
        else if (args.CurrentShapeSettings.Type == ShapeType.Rectangle)
        {
            args.CurrentShapeSettings.StrokeColor = "red";
            args.CurrentShapeSettings.StrokeWidth = 3;
        }
    }
    
    // Cancel if needed
    // args.Cancel = true;
}
```

**ShapeChangeEventArgs Properties:**
- `Action` (string): "insert", "select", "drag", "resize"
- `CurrentShapeSettings` (ShapeSettings): Shape properties
- `PreviousShapeSettings` (ShapeSettings): Previous state
- `Cancel` (bool): Cancel the operation

### ShapeChanged Event

Triggered after shape modification is completed.

```csharp
private void OnShapeChanged(ShapeChangedEventArgs args)
{
    Console.WriteLine("Shape updated");
    Console.WriteLine($"Shape ID: {args.CurrentShapeSettings.ID}");
    
    // Log changes
    // Update UI
}
```

**ShapeChangedEventArgs Properties:**
- `CurrentShapeSettings` (ShapeSettings): Updated shape properties
- `PreviousShapeSettings` (ShapeSettings): Previous state

### OnShapeResizeStart Event

Triggered when shape resizing begins.

```csharp
private void OnShapeResizeStart(ShapeChangeEventArgs args)
{
    Console.WriteLine($"Resizing shape: {args.CurrentShapeSettings.ID}");
    
    // Show resize handles
    // Lock aspect ratio if needed
}
```

### OnShapeResizeEnd Event

Triggered when shape resizing ends.

```csharp
private void OnShapeResizeEnd(ShapeChangeEventArgs args)
{
    Console.WriteLine($"Resize complete: {args.CurrentShapeSettings.Width}x{args.CurrentShapeSettings.Height}");
    
    // Update UI
    // Save changes
}
```

### OnShapeDragStart Event

Triggered when shape dragging begins.

```csharp
private void OnShapeDragStart(ShapeChangeEventArgs args)
{
    Console.WriteLine($"Dragging shape: {args.CurrentShapeSettings.ID}");
    
    // Show drag cursor
    // Highlight drop zones
}
```

### OnShapeDragEnd Event

Triggered when shape dragging ends.

```csharp
private void OnShapeDragEnd(ShapeChangeEventArgs args)
{
    Console.WriteLine($"Drop position: ({args.CurrentShapeSettings.StartX}, {args.CurrentShapeSettings.StartY})");
    
    // Validate position
    // Update layout
}
```

## Adjustment Events

### ImageFiltering Event

Triggered before a filter is applied.

```csharp
private void OnImageFiltering(ImageFilterEventArgs args)
{
    Console.WriteLine($"Applying filter: {args.Filter}");
    
    // Validate filter
    // Show preview
    
    // Cancel if needed
    // args.Cancel = true;
}
```

**ImageFilterEventArgs Properties:**
- `Filter` (ImageFilterOption): Filter type
- `Cancel` (bool): Cancel the operation

### ImageFiltered Event

Triggered after a filter is applied.

```csharp
private void OnImageFiltered(ImageFilteredEventArgs args)
{
    Console.WriteLine($"Filter applied: {args.Filter}");
    
    // Update UI
    // Enable undo
}
```

**ImageFilteredEventArgs Properties:**
- `Filter` (ImageFilterOption): Applied filter

### FinetuneValueChanging Event

Triggered when finetune adjustment value is changing.

```csharp
private void OnFinetuneValueChanging(FinetuneEventArgs args)
{
    Console.WriteLine($"Adjusting {args.FinetuneOption}: {args.Value}");
    
    // Show live preview
    // Validate range
    
    // Cancel if needed
    // args.Cancel = true;
}
```

**FinetuneEventArgs Properties:**
- `FinetuneOption` (ImageFinetuneOption): Adjustment type
- `Value` (int): Adjustment value
- `Cancel` (bool): Cancel the operation

### FinetuneValueChanged Event

Triggered after finetune adjustment is applied.

```csharp
private void OnFinetuneValueChanged(FinetuneValueChangedEventArgs args)
{
    Console.WriteLine($"Applied {args.FinetuneOption}: {args.Value}");
    
    // Update UI
    // Save state
}
```

**FinetuneValueChangedEventArgs Properties:**
- `FinetuneOption` (ImageFinetuneOption): Adjustment type
- `Value` (int): Applied value

### FrameChanging Event

Triggered before a frame is applied.

```csharp
private void OnFrameChanging(FrameChangeEventArgs args)
{
    Console.WriteLine($"Applying frame: {args.FrameType}");
    
    // Validate frame
    // Show preview
    
    // Cancel if needed
    // args.Cancel = true;
}
```

**FrameChangeEventArgs Properties:**
- `FrameType` (FrameType): Frame type
- `Cancel` (bool): Cancel the operation

### FrameChanged Event

Triggered after a frame is applied.

```csharp
private void OnFrameChanged(FrameChangedEventArgs args)
{
    Console.WriteLine($"Frame applied: {args.FrameType}");
    
    // Update UI
}
```

**FrameChangedEventArgs Properties:**
- `FrameType` (FrameType): Applied frame type

### ImageResizing Event

Triggered before image resize operation.

```csharp
private void OnImageResizing(ImageResizeEventArgs args)
{
    Console.WriteLine($"Resizing to: {args.Width}x{args.Height}");
    
    // Validate dimensions
    // Check aspect ratio
    
    // Cancel if needed
    // args.Cancel = true;
}
```

**ImageResizeEventArgs Properties:**
- `Width` (int): Target width
- `Height` (int): Target height
- `IsAspectRatio` (bool): Maintain aspect ratio
- `Cancel` (bool): Cancel the operation

### ImageResized Event

Triggered after image is resized.

```csharp
private void OnImageResized(ImageResizedEventArgs args)
{
    Console.WriteLine($"Resized to: {args.Width}x{args.Height}");
    
    // Update UI
}
```

**ImageResizedEventArgs Properties:**
- `Width` (int): New width
- `Height` (int): New height

## User Interaction Events

### Zooming Event

Triggered before zoom operation.

```csharp
private void OnZooming(ZoomEventArgs args)
{
    Console.WriteLine($"Zoom level: {args.ZoomFactor}");
    Console.WriteLine($"Zoom trigger: {args.ZoomTrigger}");
    
    // Validate zoom level
    // Show zoom indicator
    
    // Cancel if needed
    // args.Cancel = true;
}
```

**ZoomEventArgs Properties:**
- `ZoomFactor` (double): Zoom level
- `ZoomTrigger` (ZoomTrigger): How zoom was triggered (Toolbar, MouseWheel, Pinch, etc.)
- `Cancel` (bool): Cancel the operation

### Zoomed Event

Triggered after zoom operation completes.

```csharp
private void OnZoomed(ZoomedEventArgs args)
{
    Console.WriteLine($"Zoomed to: {args.ZoomFactor}");
    
    // Update zoom controls
}
```

**ZoomedEventArgs Properties:**
- `ZoomFactor` (double): Applied zoom level
- `ZoomTrigger` (ZoomTrigger): Zoom method used

### OnPanStart Event

Triggered when image panning starts.

```csharp
private void OnPanStart(PanEventArgs args)
{
    Console.WriteLine("Pan started");
    
    // Show pan cursor
    // Disable other interactions
}
```

**PanEventArgs Properties:**
- Position information (varies by implementation)

### OnPanEnd Event

Triggered when image panning ends.

```csharp
private void OnPanEnd(PanEventArgs args)
{
    Console.WriteLine("Pan ended");
    
    // Restore cursor
    // Re-enable interactions
}
```

### OnSelectionResizeStart Event

Triggered when selection resizing starts (crop mode).

```csharp
private void OnSelectionResizeStart(SelectionChangeEventArgs args)
{
    Console.WriteLine("Selection resize started");
    
    // Show resize handles
}
```

**SelectionChangeEventArgs Properties:**
- Selection position and dimension information

### OnSelectionResizeEnd Event

Triggered when selection resizing ends.

```csharp
private void OnSelectionResizeEnd(SelectionChangeEventArgs args)
{
    Console.WriteLine("Selection resize ended");
    
    // Validate selection size
}
```

### Clicked Event

Triggered when the image editor canvas is clicked.

```csharp
private void OnClicked(ImageEditorClickEventArgs args)
{
    Console.WriteLine($"Clicked at: ({args.X}, {args.Y})");
    
    // Handle click actions
    // Show context menu
    // Custom interactions
}
```

**ImageEditorClickEventArgs Properties:**
- `X` (double): Click X-coordinate
- `Y` (double): Click Y-coordinate

## Toolbar Events

### ToolbarUpdating Event

Triggered when contextual toolbar is about to be displayed.

```csharp
private void OnToolbarUpdating(ToolbarEventArgs args)
{
    Console.WriteLine($"Toolbar type: {args.ToolbarType}");
    
    // Customize toolbar items
    if (args.ToolbarType == ShapeType.Text)
    {
        // Show only specific items for text
        args.ToolbarItems = new List<ImageEditorToolbarItemModel>()
        {
            new ImageEditorToolbarItemModel { Name = "FontSize" },
            new ImageEditorToolbarItemModel { Name = "Bold" },
            new ImageEditorToolbarItemModel { Name = "Color" }
        };
    }
}
```

**ToolbarEventArgs Properties:**
- `ToolbarType` (ShapeType): Annotation type triggering toolbar
- `ToolbarItems` (List<ImageEditorToolbarItemModel>): Toolbar items

### ToolbarItemClicked Event

Triggered when a toolbar item is clicked.

```csharp
private async void OnToolbarItemClicked(ClickEventArgs args)
{
    Console.WriteLine($"Toolbar item clicked: {args.Item.Text}");
    
    // Handle custom toolbar items
    if (args.Item.Text == "CustomRotate")
    {
        await ImageEditor.RotateAsync(45);
    }
}
```

**ClickEventArgs Properties:**
- `Item` (ImageEditorToolbarItemModel): Clicked toolbar item

### QuickAccessToolbarOpening Event

Triggered when quick access toolbar is opening (when shape is selected).

```csharp
private void OnQuickAccessToolbarOpening(QuickAccessToolbarEventArgs args)
{
    Console.WriteLine("Quick access toolbar opening");
    
    // Customize quick access options
    // Hide/show specific options
    
    // Cancel if needed
    // args.Cancel = true;
}
```

**QuickAccessToolbarEventArgs Properties:**
- `Cancel` (bool): Cancel showing the toolbar

## History Events

### HistoryChanged Event

Triggered when undo/redo history changes.

```csharp
private void OnHistoryChanged(HistoryChangedEventArgs args)
{
    Console.WriteLine($"History action: {args.Action}");
    Console.WriteLine($"Entry action: {args.EntryAction}");
    Console.WriteLine($"Can undo: {args.CanUndo}");
    Console.WriteLine($"Can redo: {args.CanRedo}");
    
    // Update undo/redo buttons
    // Show history panel
}
```

**HistoryChangedEventArgs Properties:**
- `Action` (HistoryChangedAction): Undo or Redo
- `EntryAction` (HistoryEntryAction): Type of action in history
- `CanUndo` (bool): Whether undo is available
- `CanRedo` (bool): Whether redo is available

## Event Arguments Reference

### Complete Event Arguments Classes

```csharp
// File Operations
public class FileOpenEventArgs
{
    public string FileName { get; set; }
    public long FileSize { get; set; }
    public ImageEditorFileType FileType { get; set; }
}

public class SaveEventArgs
{
    public string FileName { get; set; }
    public ImageEditorFileType FileType { get; set; }
    public double ImageQuality { get; set; }
    public bool Cancel { get; set; }
}

// Transformations
public class CropEventArgs
{
    public double StartX { get; set; }
    public double StartY { get; set; }
    public double Width { get; set; }
    public double Height { get; set; }
    public bool Cancel { get; set; }
}

public class CroppedEventArgs
{
    public double Width { get; set; }
    public double Height { get; set; }
}

public class RotateEventArgs
{
    public int Degree { get; set; }
    public bool Cancel { get; set; }
}

public class RotatedEventArgs
{
    public int Degree { get; set; }
}

public class FlipEventArgs
{
    public ImageEditorDirection Direction { get; set; }
    public bool Cancel { get; set; }
}

public class FlippedEventArgs
{
    public ImageEditorDirection Direction { get; set; }
}

// Shapes
public class ShapeChangeEventArgs
{
    public string Action { get; set; }
    public ShapeSettings CurrentShapeSettings { get; set; }
    public ShapeSettings PreviousShapeSettings { get; set; }
    public bool Cancel { get; set; }
}

public class ShapeChangedEventArgs
{
    public ShapeSettings CurrentShapeSettings { get; set; }
    public ShapeSettings PreviousShapeSettings { get; set; }
}

// Adjustments
public class ImageFilterEventArgs
{
    public ImageFilterOption Filter { get; set; }
    public bool Cancel { get; set; }
}

public class ImageFilteredEventArgs
{
    public ImageFilterOption Filter { get; set; }
}

public class FinetuneEventArgs
{
    public ImageFinetuneOption FinetuneOption { get; set; }
    public int Value { get; set; }
    public bool Cancel { get; set; }
}

public class FinetuneValueChangedEventArgs
{
    public ImageFinetuneOption FinetuneOption { get; set; }
    public int Value { get; set; }
}

public class FrameChangeEventArgs
{
    public FrameType FrameType { get; set; }
    public bool Cancel { get; set; }
}

public class FrameChangedEventArgs
{
    public FrameType FrameType { get; set; }
}

public class ImageResizeEventArgs
{
    public int Width { get; set; }
    public int Height { get; set; }
    public bool IsAspectRatio { get; set; }
    public bool Cancel { get; set; }
}

public class ImageResizedEventArgs
{
    public int Width { get; set; }
    public int Height { get; set; }
}

// User Interactions
public class ZoomEventArgs
{
    public double ZoomFactor { get; set; }
    public ZoomTrigger ZoomTrigger { get; set; }
    public bool Cancel { get; set; }
}

public class ZoomedEventArgs
{
    public double ZoomFactor { get; set; }
    public ZoomTrigger ZoomTrigger { get; set; }
}

public class PanEventArgs
{
    // Position and delta information
}

public class SelectionChangeEventArgs
{
    // Selection dimensions and position
}

public class ImageEditorClickEventArgs
{
    public double X { get; set; }
    public double Y { get; set; }
}

// Toolbar
public class ToolbarEventArgs
{
    public ShapeType ToolbarType { get; set; }
    public List<ImageEditorToolbarItemModel> ToolbarItems { get; set; }
}

public class QuickAccessToolbarEventArgs
{
    public bool Cancel { get; set; }
}

// History
public class HistoryChangedEventArgs
{
    public HistoryChangedAction Action { get; set; }
    public HistoryEntryAction EntryAction { get; set; }
    public bool CanUndo { get; set; }
    public bool CanRedo { get; set; }
}
```

## Best Practices

### Event Handling Patterns

```csharp
// Pattern 1: Validation
private void OnCropping(CropEventArgs args)
{
    // Validate before operation
    if (args.Width < 100 || args.Height < 100)
    {
        args.Cancel = true;
        ShowMessage("Minimum crop size is 100x100");
    }
}

// Pattern 2: UI Updates
private void OnCropped(CroppedEventArgs args)
{
    // Update UI after operation
    UpdateStatusBar($"Image cropped to {args.Width}x{args.Height}");
}

// Pattern 3: Logging
private void OnHistoryChanged(HistoryChangedEventArgs args)
{
    // Log user actions
    LogAction($"User performed {args.EntryAction}");
}

// Pattern 4: Customization
private void OnShapeChanging(ShapeChangeEventArgs args)
{
    // Apply consistent styling
    if (args.Action == "insert")
    {
        ApplyBrandColors(args.CurrentShapeSettings);
    }
}
```

### Performance Considerations

```csharp
// Avoid heavy operations in frequent events
private void OnFinetuneValueChanging(FinetuneEventArgs args)
{
    // This fires continuously during slider drag
    // Keep processing lightweight
    UpdatePreviewLabel(args.Value);  // OK
    // await SaveToDatabase(args.Value);  // AVOID
}

// Use completed events for heavy operations
private async void OnFinetuneValueChanged(FinetuneValueChangedEventArgs args)
{
    // This fires once when adjustment is done
    // Safe for database operations
    await SaveAdjustment(args.FinetuneOption, args.Value);
}
```

All events provide comprehensive control over the Image Editor behavior and enable advanced customization scenarios.
