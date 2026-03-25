# Image Transformations in the Image Editor

## Overview

Image transformations include cropping, rotating, flipping, straightening, and resizing. These operations modify the image structure and are tracked in the undo/redo history.

## Cropping Images

### Selection Types

The Image Editor supports multiple crop selection types:

1. **Custom:** Arbitrary rectangular selection
2. **Circle:** Circular crop area
3. **Square:** Square crop area
4. **Ratio:** Fixed aspect ratio selections (16:9, 4:3, 1:1, etc.)

### Crop Workflow

```csharp
@using Syncfusion.Blazor.ImageEditor

<SfImageEditor @ref="ImageEditor" Height="500px">
    <ImageEditorEvents Created="OnCreated"></ImageEditorEvents>
</SfImageEditor>

@code {
    SfImageEditor ImageEditor;

    private async void OnCreated()
    {
        await ImageEditor.OpenAsync("https://example.com/image.png");
    }
}
```

**User interaction:**
1. Click Crop button on toolbar
2. Choose selection type from contextual toolbar
3. Draw selection on image (drag to adjust)
4. Pan image to position crop region
5. Click tick icon to apply crop

### Crop Selection Positioning

After creating a crop selection:
- Click and drag the selection to reposition
- Drag corners to resize
- Pan the image content within the selection using click-and-drag
- Use straighten slider for alignment

## Rotation

### Rotate Left and Right

Rotate the image in 90-degree increments:

```csharp
private async Task RotateImage()
{
    // Rotate 90 degrees clockwise
    await ImageEditor.RotateAsync(90);
    
    // Rotate 90 degrees counter-clockwise
    await ImageEditor.RotateAsync(-90);
    
    // Rotate 180 degrees
    await ImageEditor.RotateAsync(180);
}
```

### Using Toolbar Buttons

- **Rotate Left:** Rotate counter-clockwise by 90°
- **Rotate Right:** Rotate clockwise by 90°

These buttons are available in the toolbar's crop/transform section.

## Flipping

### Horizontal Flip

Flip the image left-to-right (mirror horizontally):

```csharp
private async Task FlipHorizontal()
{
    await ImageEditor.FlipAsync(ImageEditorDirection.Horizontal);
}
```

### Vertical Flip

Flip the image top-to-bottom (mirror vertically):

```csharp
private async Task FlipVertical()
{
    await ImageEditor.FlipAsync(ImageEditorDirection.Vertical);
}
```

### Toolbar Buttons

- **Horizontal Flip:** Mirrors the image left-right
- **Vertical Flip:** Mirrors the image top-bottom

Both are available in the crop/transform toolbar section.

## Straightening

### Straighten with Slider

When in crop mode, the straightening slider allows precise angle adjustment:

```csharp
// Straighten is applied through the crop toolbar
// User interaction:
// 1. Click Crop
// 2. Use straighten slider to adjust angle
// 3. Click tick to apply
```

The straighten slider:
- Adjusts image rotation in small increments
- Applies to inserted annotations as well
- Preview shows in real-time
- Committed with crop confirmation

## Resizing Images

### Resize Dimensions

Change the overall image dimensions:

```csharp
private async Task ResizeImage(int width, int height)
{
    // Parameters are in pixels
    await ImageEditor.ResizeAsync(width, height);
}

// Example: Resize to 800x600
private async Task ResizeExample()
{
    await ImageEditor.ResizeAsync(800, 600);
}
```

### Resize Workflow

Using the toolbar:
1. Click Resize button
2. Enter desired width and height
3. Aspect ratio option available
4. Apply changes

## Panning

### Pan Images

Move the image within the canvas, useful when zoomed in:

```csharp
// Panning is enabled:
// 1. When a crop selection is active
// 2. When image size exceeds canvas size (zoomed)

// User interaction:
// Click and drag on the image to pan
```

**Panning conditions:**
- Active crop selection for positioning the crop region
- Zoomed image that extends beyond canvas boundaries
- Drag gestures on touch devices

### Pan with Mouse

Click and drag the image to move it within the canvas. The cursor changes to indicate panning is available.

## Transform Collection

### Get Current Transforms

Retrieve all applied transformations:

```csharp
private async Task GetTransforms()
{
    ImageDimension dimension = await ImageEditor.GetImageDimensionAsync();
    // dimension includes current position and size after transforms
}
```

### Apply Multiple Transforms

Transformations can be combined:

```csharp
private async Task ComplexTransform()
{
    // Crop
    // Then rotate
    await ImageEditor.RotateAsync(90);
    
    // Then flip
    await ImageEditor.FlipAsync(ImageEditorDirection.Horizontal);
    
    // Then straighten (in crop mode)
    // All are tracked in undo/redo
}
```

Undo/redo respects the transformation sequence.

## Transformation Combinations

### Common Workflow Examples

**Example 1: Rotate and crop**
```
1. Rotate 90° clockwise
2. Enter crop mode
3. Select custom crop area
4. Apply crop
5. Can undo any step
```

**Example 2: Straighten and resize**
```
1. Enter crop mode
2. Use straighten slider
3. Apply crop
4. Resize image
5. Export
```

**Example 3: Flip and adjust**
```
1. Flip horizontally
2. Enter crop mode
3. Position crop region
4. Apply crop
5. Undo if needed
```

## Best Practices

- Use crop for precision: custom selections for exact areas, ratios for maintaining proportions
- Rotate before cropping for better alignment
- Use straighten slider for fine-tuning angles
- Pan to verify crop region before applying
- Test transformations with undo/redo before export
- Resize last to optimize final image dimensions

All transformations support undo/redo and do not permanently modify the source image until exported.

## Straightening with Precise Angles

### StraightenImageAsync Method

Apply precise straightening angles programmatically (beyond the crop toolbar slider):

`csharp
Task<bool> StraightenImageAsync(int degree)
`

**Parameters:**
- **degree** (int): Rotation angle between -45 and +45 degrees
  - Positive values: Clockwise rotation
  - Negative values: Counter-clockwise rotation

### Programmatic Straightening

`csharp
private async Task StraightenImage(int angle)
{
    // Validate angle range
    if (angle < -45 || angle > 45)
    {
        Console.WriteLine("Angle must be between -45 and +45 degrees");
        return;
    }
    
    // Apply straightening
    bool success = await ImageEditor.StraightenImageAsync(angle);
    
    if (success)
    {
        Console.WriteLine(Straightened by {angle} degrees);
    }
}

// Examples:
private async Task StraightenExamples()
{
    // Slight counter-clockwise adjustment
    await ImageEditor.StraightenImageAsync(-5);
    
    // Moderate clockwise adjustment
    await ImageEditor.StraightenImageAsync(10);
    
    // Maximum counter-clockwise
    await ImageEditor.StraightenImageAsync(-45);
    
    // Maximum clockwise
    await ImageEditor.StraightenImageAsync(45);
}
`

### Use Cases

**Straighten scanned documents:**
`csharp
private async Task StraightenDocument()
{
    // Common angles for scanned document correction
    await ImageEditor.StraightenImageAsync(-2);  // Slight tilt correction
}
`

**Fix horizon in photos:**
`csharp
private async Task FixHorizon()
{
    // Correct tilted horizon lines
    await ImageEditor.StraightenImageAsync(3);
}
`

**Precise architectural photo correction:**
`csharp
private async Task CorrectArchitecture()
{
    // Fine-tune building alignment
    await ImageEditor.StraightenImageAsync(-1);
}
`

### Difference from RotateAsync

| Method | Angle Range | Use Case |
|--------|-------------|----------|
| RotateAsync() | 0, 90, 180, 270, 360 | 90-degree rotations |
| StraightenImageAsync() | -45 to +45 degrees | Fine adjustments and leveling |

### Straightening Workflow

`csharp
// 1. Load image
await ImageEditor.OpenAsync("tilted-photo.jpg");

// 2. Apply straightening
await ImageEditor.StraightenImageAsync(-3);

// 3. Crop to remove gaps if needed
await ImageEditor.SelectAsync("custom", 0, 0, 800, 600);
await ImageEditor.CropAsync();

// 4. Export
await ImageEditor.ExportAsync("straightened-photo.jpg");
`

**Note:** Straightening creates blank areas at corners. Consider cropping after straightening to remove these areas.
