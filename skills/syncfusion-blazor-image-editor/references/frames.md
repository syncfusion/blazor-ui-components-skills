# Frames in the Image Editor

## Overview

Frames add decorative borders and visual effects around images, enhancing their presentation. The Image Editor provides various frame types with extensive customization options including colors, gradients, sizes, and styles.

## Table of Contents
- [Drawing Frames](#drawing-frames)
- [Frame Types](#frame-types)
- [Frame Customization](#frame-customization)
- [Frame Styles](#frame-styles)
- [Practical Examples](#practical-examples)

## Drawing Frames

### DrawFrameAsync Method

The `DrawFrameAsync` method applies customizable frames to images.

**Method Signature:**
```csharp
Task<bool> DrawFrameAsync(
    FrameType frameType,
    string color = "#fff",
    string gradientColor = "",
    int size = 20,
    int inset = -1,
    int offset = -1,
    int borderRadius = -1,
    FrameLineStyle frameLineStyle = FrameLineStyle.Solid,
    int lineCount = -1
)
```

**Parameters:**
- **frameType** (FrameType): Type of frame - Mat, Bevel, Line, Inset, or Hook
- **color** (string): Primary frame color (default: "#fff" white)
- **gradientColor** (string): Secondary color for gradient effects (default: empty)
- **size** (int): Frame size as percentage of image dimensions (default: 20)
- **inset** (int): Inset value for Line, Hook, and Inset frames as percentage (default: 0)
- **offset** (int): Offset value for Line and Inset frames as percentage (default: 0)
- **borderRadius** (int): Border radius for Line frames as percentage (default: 0)
- **frameLineStyle** (FrameLineStyle): Line style for Line frames - Solid, Dashed, or Dotted (default: Solid)
- **lineCount** (int): Number of lines for Line frame type (default: 0)

### Basic Frame Example

```csharp
@using Syncfusion.Blazor.ImageEditor

<SfImageEditor @ref="ImageEditor" Height="500px" Toolbar="@CustomToolbar">
    <ImageEditorEvents Created="OnCreated"></ImageEditorEvents>
</SfImageEditor>

@code {
    SfImageEditor ImageEditor;
    
    private List<ImageEditorToolbarItemModel> CustomToolbar = new()
    {
        new ImageEditorToolbarItemModel { Name = "Open" },
        new ImageEditorToolbarItemModel { Name = "Frame" },
        new ImageEditorToolbarItemModel { Name = "Save" }
    };

    private async void OnCreated()
    {
        await ImageEditor.OpenAsync("https://example.com/photo.jpg");
    }

    private async Task AddSimpleFrame()
    {
        // Apply a simple white mat frame
        await ImageEditor.DrawFrameAsync(
            frameType: FrameType.Mat,
            color: "#FFFFFF",
            size: 20
        );
    }
}
```

## Frame Types

### FrameType Enum

The `FrameType` enum defines available frame styles:

```csharp
public enum FrameType
{
    Mat,      // Solid border frame (like photo mat)
    Bevel,    // 3D beveled edge frame
    Line,     // Single or multiple line frame
    Inset,    // Recessed/inset frame effect
    Hook      // Corner hook frame style
}
```

### Mat Frame

Creates a solid border around the image, similar to a traditional photo mat:

```csharp
private async Task ApplyMatFrame()
{
    // Simple white mat
    await ImageEditor.DrawFrameAsync(
        FrameType.Mat,
        color: "#FFFFFF",
        size: 15
    );
    
    // Colored mat
    await ImageEditor.DrawFrameAsync(
        FrameType.Mat,
        color: "#3498db",  // Blue
        size: 20
    );
    
    // Mat with gradient
    await ImageEditor.DrawFrameAsync(
        FrameType.Mat,
        color: "#2c3e50",
        gradientColor: "#3498db",
        size: 25
    );
}
```

**Mat Frame Characteristics:**
- Solid, uniform border
- Supports solid colors and gradients
- Best for classic photo presentation
- Size parameter controls border width

### Bevel Frame

Creates a 3D beveled edge effect around the image:

```csharp
private async Task ApplyBevelFrame()
{
    // Light bevel
    await ImageEditor.DrawFrameAsync(
        FrameType.Bevel,
        color: "#ECF0F1",
        size: 15
    );
    
    // Dark bevel with gradient
    await ImageEditor.DrawFrameAsync(
        FrameType.Bevel,
        color: "#34495e",
        gradientColor: "#2c3e50",
        size: 20
    );
    
    // Metallic bevel
    await ImageEditor.DrawFrameAsync(
        FrameType.Bevel,
        color: "#95a5a6",
        gradientColor: "#7f8c8d",
        size: 18
    );
}
```

**Bevel Frame Characteristics:**
- 3D raised edge appearance
- Creates depth and dimension
- Gradient enhances 3D effect
- Best for professional photography

### Line Frame

Creates single or multiple line borders around the image:

```csharp
private async Task ApplyLineFrame()
{
    // Single solid line
    await ImageEditor.DrawFrameAsync(
        FrameType.Line,
        color: "#000000",
        size: 10,
        frameLineStyle: FrameLineStyle.Solid,
        lineCount: 1
    );
    
    // Double line
    await ImageEditor.DrawFrameAsync(
        FrameType.Line,
        color: "#e74c3c",
        size: 15,
        frameLineStyle: FrameLineStyle.Solid,
        lineCount: 2,
        offset: 5
    );
    
    // Dashed line with border radius
    await ImageEditor.DrawFrameAsync(
        FrameType.Line,
        color: "#9b59b6",
        size: 12,
        frameLineStyle: FrameLineStyle.Dashed,
        lineCount: 1,
        borderRadius: 10
    );
    
    // Triple dotted lines
    await ImageEditor.DrawFrameAsync(
        FrameType.Line,
        color: "#16a085",
        size: 20,
        frameLineStyle: FrameLineStyle.Dotted,
        lineCount: 3,
        offset: 8
    );
}
```

**Line Frame Characteristics:**
- Minimalist border style
- Supports multiple parallel lines
- Three line styles: Solid, Dashed, Dotted
- Customizable spacing and border radius

### Inset Frame

Creates a recessed or inset frame effect:

```csharp
private async Task ApplyInsetFrame()
{
    // Simple inset
    await ImageEditor.DrawFrameAsync(
        FrameType.Inset,
        color: "#34495e",
        size: 18,
        inset: 5
    );
    
    // Deep inset with offset
    await ImageEditor.DrawFrameAsync(
        FrameType.Inset,
        color: "#2c3e50",
        size: 25,
        inset: 10,
        offset: 5
    );
    
    // Inset with gradient
    await ImageEditor.DrawFrameAsync(
        FrameType.Inset,
        color: "#8e44ad",
        gradientColor: "#9b59b6",
        size: 20,
        inset: 8,
        offset: 3
    );
}
```

**Inset Frame Characteristics:**
- Recessed, sunken appearance
- Creates depth into the image
- Inset parameter controls depth
- Offset adjusts shadow placement

### Hook Frame

Creates corner hook-style frame elements:

```csharp
private async Task ApplyHookFrame()
{
    // Corner hooks
    await ImageEditor.DrawFrameAsync(
        FrameType.Hook,
        color: "#c0392b",
        size: 15,
        inset: 8
    );
    
    // Decorative hooks
    await ImageEditor.DrawFrameAsync(
        FrameType.Hook,
        color: "#f39c12",
        size: 20,
        inset: 12
    );
    
    // Gradient hooks
    await ImageEditor.DrawFrameAsync(
        FrameType.Hook,
        color: "#27ae60",
        gradientColor: "#2ecc71",
        size: 18,
        inset: 10
    );
}
```

**Hook Frame Characteristics:**
- Corner-only frame elements
- Vintage photo album aesthetic
- Inset controls hook size
- Minimal visual intrusion

## Frame Customization

### Color Customization

Frames support various color formats:

```csharp
// Named colors
await ImageEditor.DrawFrameAsync(FrameType.Mat, "white", "", 20);
await ImageEditor.DrawFrameAsync(FrameType.Mat, "black", "", 20);

// Hex colors
await ImageEditor.DrawFrameAsync(FrameType.Mat, "#3498db", "", 20);
await ImageEditor.DrawFrameAsync(FrameType.Mat, "#e74c3c", "", 20);

// RGB colors
await ImageEditor.DrawFrameAsync(FrameType.Mat, "rgb(52, 152, 219)", "", 20);

// RGBA colors (with transparency)
await ImageEditor.DrawFrameAsync(FrameType.Mat, "rgba(52, 152, 219, 0.8)", "", 20);
```

### Gradient Effects

Apply gradient colors for enhanced visual appeal:

```csharp
private async Task ApplyGradientFrames()
{
    // Warm gradient
    await ImageEditor.DrawFrameAsync(
        FrameType.Mat,
        color: "#ff6b6b",
        gradientColor: "#feca57",
        size: 22
    );
    
    // Cool gradient
    await ImageEditor.DrawFrameAsync(
        FrameType.Bevel,
        color: "#48dbfb",
        gradientColor: "#0abde3",
        size: 20
    );
    
    // Dark gradient
    await ImageEditor.DrawFrameAsync(
        FrameType.Mat,
        color: "#2c2c54",
        gradientColor: "#474787",
        size: 25
    );
}
```

### Size Adjustment

Frame size is specified as a percentage of image dimensions:

```csharp
// Thin frame (10-15%)
await ImageEditor.DrawFrameAsync(FrameType.Mat, "#FFFFFF", "", 10);

// Medium frame (15-25%)
await ImageEditor.DrawFrameAsync(FrameType.Mat, "#FFFFFF", "", 20);

// Thick frame (25-40%)
await ImageEditor.DrawFrameAsync(FrameType.Mat, "#FFFFFF", "", 30);

// Extra thick frame (40%+)
await ImageEditor.DrawFrameAsync(FrameType.Mat, "#FFFFFF", "", 45);
```

**Size Guidelines:**
- **10-15%**: Subtle, minimal border
- **15-25%**: Standard frame size
- **25-35%**: Bold, prominent border
- **35%+**: Statement frame (may overwhelm image)

## Frame Styles

### FrameLineStyle Enum

For Line frame types, specify the line style:

```csharp
public enum FrameLineStyle
{
    Solid,    // Continuous line
    Dashed,   // Dashed line pattern
    Dotted    // Dotted line pattern
}
```

### Line Style Examples

```csharp
// Solid line frame
await ImageEditor.DrawFrameAsync(
    FrameType.Line,
    "#000000",
    "",
    15,
    -1, -1, -1,
    FrameLineStyle.Solid,
    1
);

// Dashed line frame
await ImageEditor.DrawFrameAsync(
    FrameType.Line,
    "#e74c3c",
    "",
    15,
    -1, -1, -1,
    FrameLineStyle.Dashed,
    1
);

// Dotted line frame
await ImageEditor.DrawFrameAsync(
    FrameType.Line,
    "#3498db",
    "",
    15,
    -1, -1, -1,
    FrameLineStyle.Dotted,
    1
);
```

## Practical Examples

### Example 1: Classic Photo Mat

```csharp
private async Task CreateClassicPhotoMat()
{
    // White mat with subtle shadow effect
    await ImageEditor.DrawFrameAsync(
        FrameType.Mat,
        color: "#FFFFFF",
        gradientColor: "#F5F5F5",
        size: 22
    );
}
```

### Example 2: Modern Minimalist Frame

```csharp
private async Task CreateModernFrame()
{
    // Thin black line with rounded corners
    await ImageEditor.DrawFrameAsync(
        FrameType.Line,
        color: "#000000",
        size: 8,
        frameLineStyle: FrameLineStyle.Solid,
        lineCount: 1,
        borderRadius: 5
    );
}
```

### Example 3: Vintage Photo Album

```csharp
private async Task CreateVintageFrame()
{
    // Sepia-toned hook frame
    await ImageEditor.DrawFrameAsync(
        FrameType.Hook,
        color: "#8B4513",
        gradientColor: "#A0522D",
        size: 18,
        inset: 12
    );
}
```

### Example 4: Professional Portfolio

```csharp
private async Task CreateProfessionalFrame()
{
    // Elegant bevel with subtle gradient
    await ImageEditor.DrawFrameAsync(
        FrameType.Bevel,
        color: "#2c3e50",
        gradientColor: "#34495e",
        size: 20
    );
}
```

### Example 5: Artistic Border

```csharp
private async Task CreateArtisticFrame()
{
    // Multiple dashed lines
    await ImageEditor.DrawFrameAsync(
        FrameType.Line,
        color: "#9b59b6",
        size: 25,
        offset: 10,
        borderRadius: 8,
        frameLineStyle: FrameLineStyle.Dashed,
        lineCount: 3
    );
}
```

### Example 6: Certificate Border

```csharp
private async Task CreateCertificateBorder()
{
    // Formal double-line frame
    await ImageEditor.DrawFrameAsync(
        FrameType.Line,
        color: "#C0A062",  // Gold color
        size: 30,
        offset: 12,
        frameLineStyle: FrameLineStyle.Solid,
        lineCount: 2
    );
}
```

## ImageFrameSettings Class

The `ImageFrameSettings` class represents frame configuration:

```csharp
public class ImageFrameSettings
{
    public FrameType Type { get; set; }              // Frame type
    public string Color { get; set; }                // Primary color
    public string GradientColor { get; set; }        // Gradient color
    public int Size { get; set; }                    // Size percentage
    public int Inset { get; set; }                   // Inset value
    public int Offset { get; set; }                  // Offset value
    public int BorderRadius { get; set; }            // Border radius
    public FrameLineStyle LineStyle { get; set; }    // Line style
    public int LineCount { get; set; }               // Number of lines
}
```

## Best Practices

### Frame Selection by Image Type

```csharp
// Portraits: Use mat or bevel frames
await ImageEditor.DrawFrameAsync(FrameType.Mat, "#FFFFFF", "", 20);

// Landscapes: Use line or hook frames
await ImageEditor.DrawFrameAsync(FrameType.Line, "#000000", "", 12);

// Product photos: Use clean, minimal frames
await ImageEditor.DrawFrameAsync(FrameType.Line, "#E0E0E0", "", 10);

// Artistic photos: Use creative frames with gradients
await ImageEditor.DrawFrameAsync(FrameType.Bevel, "#3498db", "#9b59b6", 25);
```

### Color Harmony

```csharp
// Neutral frames for any image
await ImageEditor.DrawFrameAsync(FrameType.Mat, "#FFFFFF", "", 20);
await ImageEditor.DrawFrameAsync(FrameType.Mat, "#000000", "", 20);
await ImageEditor.DrawFrameAsync(FrameType.Mat, "#808080", "", 20);

// Complementary colors enhance subject
// For warm-toned images, use cool frames
await ImageEditor.DrawFrameAsync(FrameType.Mat, "#3498db", "#2980b9", 22);

// For cool-toned images, use warm frames
await ImageEditor.DrawFrameAsync(FrameType.Mat, "#e74c3c", "#c0392b", 22);
```

### Size Proportions

```csharp
// Square images: Medium frames work well
await ImageEditor.DrawFrameAsync(FrameType.Mat, "#FFFFFF", "", 20);

// Landscape images: Thinner frames on sides
await ImageEditor.DrawFrameAsync(FrameType.Mat, "#FFFFFF", "", 15);

// Portrait images: Slightly larger frames
await ImageEditor.DrawFrameAsync(FrameType.Mat, "#FFFFFF", "", 25);
```

### Performance Tips

```csharp
// Apply frame last in editing workflow
// 1. Load image
await ImageEditor.OpenAsync("photo.jpg");

// 2. Apply edits (crop, filters, annotations)
await ImageEditor.CropAsync();
await ImageEditor.ApplyImageFilterAsync(ImageFilterOption.Sepia);

// 3. Apply frame as final step
await ImageEditor.DrawFrameAsync(FrameType.Mat, "#FFFFFF", "", 20);

// 4. Export
await ImageEditor.ExportAsync("final-photo.jpg");
```

## Limitations

- Only one frame can be applied at a time
- Applying a new frame replaces the existing frame
- Frame parameters must be within valid ranges:
  - Size: 0-100 (percentage)
  - Inset: 0-50 (percentage)
  - Offset: 0-50 (percentage)
  - BorderRadius: 0-50 (percentage)
  - LineCount: 1-10 (for Line frames)

## Removing Frames

To remove a frame, use the Undo function:

```csharp
// Apply frame
await ImageEditor.DrawFrameAsync(FrameType.Mat, "#FFFFFF", "", 20);

// Remove frame using undo
await ImageEditor.UndoAsync();

// Or reset entire image
await ImageEditor.ResetAsync();
```

Frames are permanent once exported. Use preview mode to test frame appearance before exporting.

## Accessibility Considerations

- Ensure sufficient contrast between frame and image
- Test frames with screen magnification
- Consider color-blind users when choosing frame colors
- Provide alternative text descriptions for framed images

All frame operations support undo/redo and are included in the exported image.
