# User Interactions in the Image Editor

## Overview

The Image Editor supports multiple interaction methods for zooming, panning, selecting, and navigating images. All interactions provide intuitive user experience across desktop and mobile devices.

## Zooming

### Zoom Methods

Users can zoom images using four different methods:

#### 1. Toolbar Zoom Buttons

Click toolbar buttons to zoom in or out:

- **Zoom In** (+) - Increase magnification
- **Zoom Out** (−) - Decrease magnification

Buttons become enabled after image is loaded.

#### 2. Mouse Wheel

Press Ctrl and scroll to zoom:

```
Ctrl + Scroll Up → Zoom In
Ctrl + Scroll Down → Zoom Out
```

Smooth zoom with continuous scroll.

#### 3. Keyboard Shortcuts

```
Ctrl + '+' → Zoom in (increase magnification)
Ctrl + '−' → Zoom out (decrease magnification)
Ctrl + '0' → Reset to default zoom level
```

#### 4. Pinch Gesture (Touch)

On touch-enabled devices, use two-finger pinch gesture:

```
Pinch In → Zoom Out
Pinch Out → Zoom In
```

Zoom level controlled by pinch distance.

### Zoom Levels

| Action | Zoom Level |
|--------|-----------|
| Fit to window | Automatic |
| Initial load | 100% |
| Zoom in (×5) | Up to 500% |
| Zoom out (÷5) | Down to 10% |

## Panning

### When Panning is Available

Panning is enabled in two scenarios:

1. **Active Crop Selection** - Move image to position crop region
2. **Zoomed Image** - Move image within canvas when zoomed beyond canvas size

### Pan Interaction

Click and drag the image to move it:

```
Click on image + Drag → Pan in direction
```

Cursor changes to indicate panning is available.

### Pan with Arrow Keys

When zoomed, use arrow keys to pan:

```
Arrow Up → Pan up
Arrow Down → Pan down
Arrow Left → Pan left
Arrow Right → Pan right
```

Continuous arrow press pans smoothly.

## Selection Types for Cropping

### Available Selection Types

When in crop mode, choose from these selection types:

#### 1. Custom

Free-form rectangular selection:
- Click and drag to define area
- Resize by dragging corners
- Pan to reposition

#### 2. Circle

Circular crop area:
- Drag to define diameter
- Resize by dragging edge
- Maintains circular shape

#### 3. Square

Square crop area:
- Drag to define size
- Resize maintains 1:1 aspect ratio
- Adjustable through corners

#### 4. Ratio

Fixed aspect ratio selections:
- **16:9** - Widescreen
- **4:3** - Standard
- **1:1** - Square
- **3:2** - Classic film
- **Other ratios** as configured

### Using Selection Types

```csharp
@using Syncfusion.Blazor.ImageEditor

<SfImageEditor @ref="ImageEditor" Height="500px">
    <ImageEditorEvents Created="OnCreated"></ImageEditorEvents>
</SfImageEditor>

@code {
    SfImageEditor ImageEditor;

    // User workflow:
    // 1. Click Crop button
    // 2. Select desired selection type from dropdown
    // 3. Draw selection on image
    // 4. Adjust size and position
    // 5. Click tick to apply
}
```

## Image Dimensions API

### Get Image Information

Retrieve current image dimensions and coordinates:

```csharp
private async void GetImageInfo()
{
    ImageDimension Dimension = await ImageEditor.GetImageDimensionAsync();
    
    double x = Dimension.X.Value;              // X-coordinate (left position)
    double y = Dimension.Y.Value;              // Y-coordinate (top position)
    double width = Dimension.Width.Value;      // Image width in pixels
    double height = Dimension.Height.Value;    // Image height in pixels
    
    Console.WriteLine($"Position: ({x}, {y})");
    Console.WriteLine($"Size: {width}x{height}");
}
```

### Use Dimensions for Annotations

Position annotations relative to image:

```csharp
private async void PlaceAnnotationsRelative()
{
    ImageDimension Dimension = await ImageEditor.GetImageDimensionAsync();
    
    // Place text at image top-left
    await ImageEditor.DrawTextAsync(
        Dimension.X.Value, 
        Dimension.Y.Value, 
        "Label"
    );
    
    // Place at image center
    await ImageEditor.DrawTextAsync(
        Dimension.X.Value + Dimension.Width.Value / 2,
        Dimension.Y.Value + Dimension.Height.Value / 2,
        "Center Label"
    );
    
    // Place at bottom-right
    await ImageEditor.DrawTextAsync(
        Dimension.X.Value + Dimension.Width.Value - 100,
        Dimension.Y.Value + Dimension.Height.Value - 50,
        "Bottom Label"
    );
}
```

## Keyboard Shortcuts Reference

### Navigation Shortcuts

| Shortcut | Action |
|----------|--------|
| `Ctrl + +` | Zoom in |
| `Ctrl + −` | Zoom out |
| `Ctrl + 0` | Fit to window / Reset zoom |
| `Arrow Keys` | Pan image when zoomed |
| `Home` | Go to top-left corner |
| `End` | Go to bottom-right corner |

### Editing Shortcuts

| Shortcut | Action |
|----------|--------|
| `Ctrl + Z` | Undo last operation |
| `Ctrl + Y` | Redo last undone operation |
| `Ctrl + S` | Save/Export image |
| `Delete` | Delete selected annotation |
| `Escape` | Deselect current selection/annotation |
| `Enter` | Confirm action/Apply changes |

### Annotation Shortcuts

| Shortcut | Action |
|----------|--------|
| `Ctrl + Click` | Multi-select annotations |
| `Shift + Click` | Extend selection |
| `Tab` | Move to next annotation |
| `Shift + Tab` | Move to previous annotation |

## Interaction Patterns

### Pattern 1: Zoom and Pan Workflow

```
1. Load image
2. Ctrl+Scroll to zoom in to detail area
3. Arrow keys or drag to pan to region
4. Add annotations at zoomed level
5. Ctrl+0 to fit to window
6. Ctrl+S to export
```

### Pattern 2: Crop with Aspect Ratio

```
1. Click Crop button
2. Select ratio selection type (16:9, 4:3, etc.)
3. Draw initial selection
4. Drag corners to resize (maintains ratio)
5. Drag selection to reposition
6. Pan image within selection to frame it perfectly
7. Click tick to apply
```

### Pattern 3: Mobile Touch Interaction

```
1. Pinch to zoom
2. Swipe to pan
3. Double-tap to fit to screen
4. Long-press for context menu
5. Two-finger tap for options
```

### Pattern 4: Annotation Workflow

```
1. Ctrl+Scroll zoom to area of interest
2. Pan to position detail
3. Click annotation tool
4. Draw annotation
5. Drag to reposition
6. Escape to deselect
7. Ctrl+Z to undo if needed
8. Continue with more annotations
```

## Touch Gestures

### Single Touch

- **Tap** - Select/deselect, activate tools
- **Drag** - Pan image or move annotation
- **Long-press** - Open context menu

### Multi-Touch

- **Pinch** - Zoom in/out
- **Two-finger drag** - Pan
- **Two-finger tap** - Activate alternate function

## Accessibility Features

### Keyboard-Only Navigation

All functions accessible via keyboard:

```csharp
// Users can navigate entirely with keyboard
Ctrl+Tab → Move focus between tools
Arrow Keys → Navigate menus
Enter → Activate selected item
Esc → Cancel/Close dialogs
```

### Screen Reader Support

- Toolbar items announced
- Zoom level reported
- Annotation actions described
- Error messages accessible

### High Contrast

- Clear visual feedback for zoom level
- Selected items highlighted
- Buttons clearly disabled/enabled
- Text annotations readable

## Best Practices

### Desktop Users

- Primary: Toolbar buttons for discoverability
- Intermediate: Keyboard shortcuts for efficiency
- Advanced: Mouse wheel + keyboard combinations

### Mobile Users

- Optimize for touch: Larger target areas
- Support pinch zoom natively
- Simplify gestures for common tasks
- Provide fallback toolbar for complex operations

### Mixed Scenarios

- Detect input method (touch vs mouse)
- Adapt toolbar based on device
- Test across browsers and devices
- Provide consistent experience

All interactions are non-destructive and support undo/redo operations.
