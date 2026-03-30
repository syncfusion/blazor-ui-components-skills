# Annotation Styling in the Image Editor

## Overview

Customize the appearance of annotations through stroke colors, fill colors, font styling, and event-based customization. All styling changes are tracked in undo/redo history.

## Stroke Customization

### Stroke Color and Width

Customize the border appearance of shapes and freehand drawings:

```csharp
@using Syncfusion.Blazor.ImageEditor

<SfImageEditor @ref="ImageEditor" Height="500px">
    <ImageEditorEvents Created="OnCreated" ShapeChanging="OnShapeChanging"></ImageEditorEvents>
</SfImageEditor>

@code {
    SfImageEditor ImageEditor;

    private async void OnCreated()
    {
        await ImageEditor.OpenAsync("YOUR_IMAGE_URL");
    }

    private void OnShapeChanging(ShapeChangeEventArgs args)
    {
        // Customize all shapes
        args.CurrentShapeSettings.StrokeColor = "red";
        args.CurrentShapeSettings.StrokeWidth = 3;
    }
}
```

### Color Formats

Supported color formats for stroke:

- **Named colors:** "red", "blue", "green", "yellow", "black", "white", "orange", "purple", "pink"
- **Hex codes:** "#FF0000", "#00FF00", "#0000FF"
- **RGB:** "rgb(255, 0, 0)"
- **HSL:** "hsl(0, 100%, 50%)"

### Stroke Width

Set border thickness in pixels:

```csharp
private void OnShapeChanging(ShapeChangeEventArgs args)
{
    if (args.CurrentShapeSettings.Type == ShapeType.Rectangle)
    {
        args.CurrentShapeSettings.StrokeWidth = 5;  // Thicker border
    }
}
```

## Fill Colors

### Shape Fill

Set the interior color of shapes:

```csharp
private async void DrawFilledShape()
{
    ImageDimension Dimension = await ImageEditor.GetImageDimensionAsync();
    
    await ImageEditor.DrawRectangleAsync(
        x: Dimension.X.Value + 50,
        y: Dimension.Y.Value + 50,
        width: 150,
        height: 100,
        strokeWidth: 2,
        strokeColor: "black",
        fillColor: "lightblue",   // Fill interior
        degree: 0,
        isSelected: false,
        borderRadius: 5
    );
}
```

### Transparent Fill

Use transparent backgrounds:

```csharp
private void OnShapeChanging(ShapeChangeEventArgs args)
{
    if (args.CurrentShapeSettings.Type == ShapeType.Ellipse)
    {
        args.CurrentShapeSettings.FillColor = "transparent";
    }
}
```

## Font Styling

### Text Font Customization

```csharp
private async void AddStyledText()
{
    ImageDimension Dimension = await ImageEditor.GetImageDimensionAsync();
    
    await ImageEditor.DrawTextAsync(
        x: Dimension.X.Value,
        y: Dimension.Y.Value,
        text: "Styled Text",
        fontFamily: "Georgia",    // Font family
        fontSize: 36,             // Size in pixels
        bold: true,               // Bold style
        italic: true,             // Italic style
        color: "darkblue"         // Text color
    );
}
```

### Font Family Options

Available font families (default):
- Arial
- Times New Roman
- Courier New
- Georgia
- Verdana

### Custom Font Families

Add additional fonts using `ImageEditorFontFamily`:

```csharp
<SfImageEditor @ref="ImageEditor" Height="400">
    <ImageEditorFontFamily Items="@CustomItems" Default="Arial"></ImageEditorFontFamily>
    <ImageEditorEvents Created="OnCreated"></ImageEditorEvents>
</SfImageEditor>

@code {
    SfImageEditor ImageEditor;
    
    private List<ImageEditorDropDownItemModel> CustomItems = new()
    {
        new ImageEditorDropDownItemModel { Text = "Arial", Value = "arial" },
        new ImageEditorDropDownItemModel { Text = "Brush Script MT", Value = "brush script mt" },
        new ImageEditorDropDownItemModel { Text = "Papyrus", Value = "papyrus" },
        new ImageEditorDropDownItemModel { Text = "Times New Roman", Value = "times new roman" },
        new ImageEditorDropDownItemModel { Text = "Courier New", Value = "courier new" }
    };

    private async void OnCreated()
    {
        await ImageEditor.OpenAsync("YOUR_IMAGE_URL");
    }
}
```

### Font Size

Set text size in pixels:

```csharp
// Small text (12px)
await ImageEditor.DrawTextAsync(x, y, "Small", "Arial", 12);

// Medium text (24px)
await ImageEditor.DrawTextAsync(x, y, "Medium", "Arial", 24);

// Large text (48px)
await ImageEditor.DrawTextAsync(x, y, "Large", "Arial", 48);
```

## ShapeChanging Event

### Event Basics

The `ShapeChanging` event fires when annotations are created, selected, or modified:

```csharp
private void OnShapeChanging(ShapeChangeEventArgs args)
{
    // args.Action: "insert", "select", "draw"
    // args.CurrentShapeSettings: Annotation properties
}
```

### Conditional Styling

Apply different styles based on shape type:

```csharp
private void OnShapeChanging(ShapeChangeEventArgs args)
{
    if (args.Action == "insert")
    {
        if (args.CurrentShapeSettings.Type == ShapeType.Text)
        {
            args.CurrentShapeSettings.Color = "darkblue";
            args.CurrentShapeSettings.FontFamily = "Arial";
            args.CurrentShapeSettings.FontSize = 24;
        }
        else if (args.CurrentShapeSettings.Type == ShapeType.Rectangle)
        {
            args.CurrentShapeSettings.StrokeColor = "black";
            args.CurrentShapeSettings.StrokeWidth = 2;
            args.CurrentShapeSettings.FillColor = "lightyellow";
        }
        else if (args.CurrentShapeSettings.Type == ShapeType.FreehandDraw)
        {
            args.CurrentShapeSettings.StrokeColor = "red";
            args.CurrentShapeSettings.StrokeWidth = 3;
        }
    }
}
```

### Set Default Stroke Colors

Modify defaults while preserving user selections:

```csharp
private void OnShapeChanging(ShapeChangeEventArgs args)
{
    // Only set defaults on insert (new shapes)
    if (args.Action == "insert")
    {
        // Set default for all new rectangles
        if (args.CurrentShapeSettings.Type == ShapeType.Rectangle)
        {
            if (string.IsNullOrEmpty(args.CurrentShapeSettings.StrokeColor))
            {
                args.CurrentShapeSettings.StrokeColor = "blue";
            }
        }
    }
}
```

## ShapeSettings Properties

### Available Properties

| Property | Type | Example | Purpose |
|----------|------|---------|---------|
| `ID` | string | "shape_1" | Unique identifier |
| `Type` | ShapeType | ShapeType.Text | Annotation type |
| `Color` | string | "red" | Text/stroke color |
| `FillColor` | string | "lightyellow" | Shape fill |
| `StrokeColor` | string | "black" | Border color |
| `StrokeWidth` | int | 3 | Border thickness |
| `FontFamily` | string | "Arial" | Text font |
| `FontSize` | int | 24 | Text size in px |
| `Bold` | bool | true | Bold text |
| `Italic` | bool | false | Italic text |
| `FontStyle` | string[] | ["bold", "underline"] | Multiple styles |

### Update Properties

Modify shapes after creation:

```csharp
private async void UpdateShapeStyle()
{
    ShapeSettings[] shapes = await ImageEditor.GetShapesAsync();
    
    if (shapes != null && shapes.Length > 0)
    {
        ShapeSettings shape = shapes[0];
        
        shape.StrokeColor = "green";
        shape.StrokeWidth = 4;
        shape.FillColor = "lightgreen";
        
        await ImageEditor.UpdateShapeAsync(shape);
    }
}
```

## Practical Examples

### Example 1: Watermark with Custom Style

```csharp
private async void AddStyledWatermark()
{
    ImageDimension Dimension = await ImageEditor.GetImageDimensionAsync();
    
    await ImageEditor.DrawTextAsync(
        x: Dimension.X.Value + 100,
        y: Dimension.Y.Value + 100,
        text: "© 2024 Company",
        fontFamily: "Georgia",
        fontSize: 40,
        bold: true,
        italic: false,
        color: "white",
        isSelected: false,
        degree: -45,              // Rotated
        fillColor: "rgba(0,0,0,0.3)",
        strokeColor: "black",
        strokeWidth: 1
    );
}
```

### Example 2: Contextual Shape Styling

```csharp
private void OnShapeChanging(ShapeChangeEventArgs args)
{
    if (args.Action == "insert")
    {
        // Mark important areas with red
        if (args.CurrentShapeSettings.Type == ShapeType.Ellipse)
        {
            args.CurrentShapeSettings.StrokeColor = "red";
            args.CurrentShapeSettings.StrokeWidth = 3;
            args.CurrentShapeSettings.FillColor = "transparent";
        }
        
        // Label areas with blue text
        if (args.CurrentShapeSettings.Type == ShapeType.Text)
        {
            args.CurrentShapeSettings.Color = "blue";
            args.CurrentShapeSettings.FontSize = 20;
        }
    }
}
```

### Example 3: Consistent Brand Colors

```csharp
private const string BRAND_BLUE = "#0052CC";
private const string BRAND_GRAY = "#F0F0F0";

private void OnShapeChanging(ShapeChangeEventArgs args)
{
    if (args.Action == "insert")
    {
        args.CurrentShapeSettings.StrokeColor = BRAND_BLUE;
        args.CurrentShapeSettings.FillColor = BRAND_GRAY;
        args.CurrentShapeSettings.StrokeWidth = 2;
    }
}
```

All styling changes are immediately visible in the editor and tracked in undo/redo history.
