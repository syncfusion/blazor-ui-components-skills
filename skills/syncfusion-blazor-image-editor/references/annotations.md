# Annotations in the Image Editor

## Table of Contents
- [Text Annotations](#text-annotations)
- [Multiline Text](#multiline-text)
- [Text Formatting](#text-formatting)
- [Freehand Drawing](#freehand-drawing)
- [Shape Annotations](#shape-annotations)
- [Image Annotations](#image-annotations)
- [Managing Annotations](#managing-annotations)

## Text Annotations

### Add Text

Insert text annotations at specific coordinates using `DrawTextAsync`:

```csharp
@using Syncfusion.Blazor.ImageEditor

<SfImageEditor @ref="ImageEditor" Height="500px">
    <ImageEditorEvents Created="OnCreated"></ImageEditorEvents>
</SfImageEditor>

@code {
    SfImageEditor ImageEditor;

    private async void OnCreated()
    {
        await ImageEditor.OpenAsync("YOUR_IMAGE_URL");
    }

    private async void AddSimpleText()
    {
        ImageDimension Dimension = await ImageEditor.GetImageDimensionAsync();
        // DrawTextAsync(x, y, text)
        await ImageEditor.DrawTextAsync(Dimension.X.Value, Dimension.Y.Value, "Syncfusion");
    }
}
```

### Text Customization Options

The `DrawTextAsync` method supports extensive customization:

```csharp
private async void AddCustomText()
{
    ImageDimension Dimension = await ImageEditor.GetImageDimensionAsync();
    
    await ImageEditor.DrawTextAsync(
        x: Dimension.X.Value + 50,
        y: Dimension.Y.Value + 50,
        text: "Custom Text",
        fontFamily: "Arial",          // Font family
        fontSize: 40,                 // Font size in pixels
        bold: true,                   // Bold style
        italic: false,                // Italic style
        color: "red",                 // Text color
        isSelected: false,            // Selection state
        degree: 0,                    // Rotation angle
        fillColor: "yellow",          // Background color
        strokeColor: "blue",          // Outline/border color
        strokeWidth: 2                // Border width in pixels
    );
}
```

### Text Color and Styling

```csharp
private async void AddColoredText()
{
    ImageDimension Dimension = await ImageEditor.GetImageDimensionAsync();
    
    // Red text with yellow background and blue outline
    await ImageEditor.DrawTextAsync(
        Dimension.X.Value, Dimension.Y.Value, "Important",
        "Times New Roman", 32, false, false,
        "red",      // Text color
        false, 0, "yellow", "blue", 3
    );
}
```

**Color Format:** Supported formats include:
- Named colors: "red", "blue", "green", "yellow", "black", "white", etc.
- Hex codes: "#FF0000", "#00FF00", "#0000FF"
- RGB: "rgb(255, 0, 0)"

## Multiline Text

### Add Multiline Annotations

Create text annotations spanning multiple lines using newline characters (`\n`):

```csharp
private async void AddMultilineText()
{
    ImageDimension Dimension = await ImageEditor.GetImageDimensionAsync();
    
    string multilineText = "Line 1\nLine 2\nLine 3";
    
    await ImageEditor.DrawTextAsync(
        Dimension.X.Value, Dimension.Y.Value, 
        multilineText,
        "Arial", 24, false, false, "black"
    );
}
```

**Output:** Each line separated by newline character renders on its own line.

## Text Formatting

### Bold, Italic, Underline, Strikethrough

Apply text formatting styles:

```csharp
private async void AddFormattedText()
{
    ImageDimension Dimension = await ImageEditor.GetImageDimensionAsync();
    
    // Bold text
    await ImageEditor.DrawTextAsync(
        Dimension.X.Value, Dimension.Y.Value + 10,
        "Bold Text",
        "Arial", 28, true, false  // bold=true, italic=false
    );
    
    // Italic text
    await ImageEditor.DrawTextAsync(
        Dimension.X.Value, Dimension.Y.Value + 50,
        "Italic Text",
        "Arial", 28, false, true  // bold=false, italic=true
    );
}
```

### Update Formatting After Creation

Modify formatting using `ShapeChanging` event:

```csharp
<SfImageEditor @ref="ImageEditor" Height="500px">
    <ImageEditorEvents Created="OnCreated" ShapeChanging="OnShapeChanging"></ImageEditorEvents>
</SfImageEditor>

@code {
    SfImageEditor ImageEditor;

    private void OnShapeChanging(ShapeChangeEventArgs args)
    {
        if (args.CurrentShapeSettings.Type == ShapeType.Text)
        {
            args.CurrentShapeSettings.FontStyle = new[] { "bold", "underline" };
        }
    }
}
```

## Freehand Drawing

### Enable Freehand Draw Mode

Activate freehand drawing to sketch directly on the image:

```csharp
@using Syncfusion.Blazor.ImageEditor
@using Syncfusion.Blazor.Buttons

<div>
    <SfButton OnClick="EnableFreehand">Enable Freehand Draw</SfButton>
    <SfButton OnClick="DisableFreehand">Disable Freehand Draw</SfButton>
</div>
<SfImageEditor @ref="ImageEditor" Height="500px">
    <ImageEditorEvents Created="OnCreated"></ImageEditorEvents>
</SfImageEditor>

@code {
    SfImageEditor ImageEditor;

    private async void OnCreated()
    {
        await ImageEditor.OpenAsync("YOUR_IMAGE_URL");
    }

    private async void EnableFreehand()
    {
        await ImageEditor.EnableFreehandDrawAsync();
    }

    private async void DisableFreehand()
    {
        await ImageEditor.DisableFreehandDrawAsync();
    }
}
```

### Customize Freehand Stroke

```csharp
private void OnShapeChanging(ShapeChangeEventArgs args)
{
    if (args.CurrentShapeSettings.Type == ShapeType.FreehandDraw)
    {
        args.CurrentShapeSettings.StrokeColor = "red";
        args.CurrentShapeSettings.StrokeWidth = 3;
    }
}
```

## Shape Annotations

### Rectangle

```csharp
private async void DrawRectangle()
{
    ImageDimension Dimension = await ImageEditor.GetImageDimensionAsync();
    
    await ImageEditor.DrawRectangleAsync(
        x: Dimension.X.Value + 10,
        y: Dimension.Y.Value + 60,
        width: 150,
        height: 70,
        strokeWidth: 2,
        strokeColor: "black",
        fillColor: "transparent",
        degree: 0,
        isSelected: false,
        borderRadius: 5  // Rounded corners
    );
}
```

### Ellipse

```csharp
private async void DrawEllipse()
{
    ImageDimension Dimension = await ImageEditor.GetImageDimensionAsync();
    
    await ImageEditor.DrawEllipseAsync(
        x: Dimension.X.Value,                    // Center X
        y: Dimension.Y.Value + 200,              // Center Y
        radiusX: 80,                             // Horizontal radius
        radiusY: 50,                             // Vertical radius
        strokeWidth: 2,
        strokeColor: "blue",
        fillColor: "lightblue",
        degree: 0,
        isSelected: false
    );
}
```

### Line

```csharp
private async void DrawLine()
{
    ImageDimension Dimension = await ImageEditor.GetImageDimensionAsync();
    
    await ImageEditor.DrawLineAsync(
        startX: Dimension.X.Value + 100,
        startY: Dimension.Y.Value + 50,
        endX: Dimension.X.Value + 300,
        endY: Dimension.Y.Value + 50,
        strokeWidth: 3,
        strokeColor: "green",
        isSelected: false
    );
}
```

### Arrow

```csharp
private async void DrawArrow()
{
    ImageDimension Dimension = await ImageEditor.GetImageDimensionAsync();
    
    await ImageEditor.DrawArrowAsync(
        startX: Dimension.X.Value + 150,
        startY: Dimension.Y.Value + 150,
        endX: Dimension.X.Value + 300,
        endY: Dimension.Y.Value + 150,
        strokeWidth: 3,
        strokeColor: "red",
        arrowStart: ImageEditorArrowHeadType.None,     // No arrow at start
        arrowEnd: ImageEditorArrowHeadType.SolidArrow,  // Arrow at end
        isSelected: false
    );
}
```

### Path

```csharp
private async Task DrawPath()
{
    ImageDimension dimension = await ImageEditor.GetImageDimensionAsync();
    
    ImageEditorPoint[] points = new ImageEditorPoint[]
    {
        new ImageEditorPoint { X = dimension.X.Value, Y = dimension.Y.Value },
        new ImageEditorPoint { X = dimension.X.Value + 50, Y = dimension.Y.Value + 50 },
        new ImageEditorPoint { X = dimension.X.Value + 20, Y = dimension.Y.Value + 50 }
    };
    
    await ImageEditor.DrawPathAsync(
        points: points,
        strokeWidth: 3,
        strokeColor: "purple",
        isSelected: false
    );
}
```

## Image Annotations

### Add Image Overlay

Insert images, logos, or watermarks:

```csharp
private async void AddImageAnnotation()
{
    ImageDimension Dimension = await ImageEditor.GetImageDimensionAsync();
    
    await ImageEditor.DrawImageAsync(
        data: "YOUR_IMAGE_URL",  // Image URL or data
        x: Dimension.X.Value,
        y: Dimension.Y.Value,
        width: 200,
        height: 200,
        isAspectRatio: true,   // Maintain aspect ratio
        degree: 0,             // Rotation
        opacity: 1.0,          // Opacity (0-1)
        isSelected: false
    );
}
```

## Managing Annotations

### Delete Annotations

Remove specific annotations by ID:

```csharp
private async void DeleteAnnotation()
{
    // Get all annotations
    ShapeSettings[] shapes = await ImageEditor.GetShapesAsync();
    
    if (shapes != null && shapes.Length > 0)
    {
        // Delete first annotation
        await ImageEditor.DeleteShapeAsync(shapes[0].ID);
    }
}
```

Each annotation has a unique ID automatically assigned.

### Get All Annotations

```csharp
private async void ListAnnotations()
{
    ShapeSettings[] shapes = await ImageEditor.GetShapesAsync();
    
    foreach (var shape in shapes)
    {
        // Access shape properties
        var type = shape.Type;      // ShapeType (Text, Rectangle, etc.)
        var id = shape.ID;          // Unique identifier
        var color = shape.Color;    // Color property
    }
}
```

### Update Annotations

Modify annotation properties:

```csharp
private async void UpdateAnnotation()
{
    ShapeSettings[] shapes = await ImageEditor.GetShapesAsync();
    
    if (shapes != null && shapes.Length > 0)
    {
        shapes[0].StrokeColor = "red";
        shapes[0].StrokeWidth = 5;
        
        await ImageEditor.UpdateShapeAsync(shapes[0]);
    }
}
```

## Annotation Events

### ShapeChanging Event

Triggered when annotations are modified:

```csharp
private void OnShapeChanging(ShapeChangeEventArgs args)
{
    // args.Action: "insert", "select", "draw"
    // args.CurrentShapeSettings: Current annotation properties
    
    if (args.Action == "insert")
    {
        // New annotation being added
    }
}
```

All annotations are tracked in undo/redo history.

## Advanced Annotation Methods

### DrawTextAsync with Complete Parameters

The full DrawTextAsync method supports additional text formatting:

`csharp
Task<bool> DrawTextAsync(
    double x = -1,
    double y = -1,
    string text = "",
    string fontFamily = "",
    int fontSize = -1,
    bool bold = false,
    bool italic = false,
    string color = "",
    bool isSelected = false,
    int degree = -1,
    string fillColor = "",
    string strokeColor = "",
    int strokeWidth = -1,
    bool underline = false,       // NEW
    bool strikethrough = false    // NEW
)
`

### Text with Underline and Strikethrough

`csharp
private async void AddFormattedText()
{
    ImageDimension Dimension = await ImageEditor.GetImageDimensionAsync();
    
    // Underlined text
    await ImageEditor.DrawTextAsync(
        x: Dimension.X.Value,
        y: Dimension.Y.Value + 10,
        text: "Underlined Text",
        fontFamily: "Arial",
        fontSize: 24,
        bold: false,
        italic: false,
        color: "black",
        isSelected: false,
        degree: 0,
        fillColor: "",
        strokeColor: "",
        strokeWidth: -1,
        underline: true,             // Underline
        strikethrough: false
    );
    
    // Strikethrough text
    await ImageEditor.DrawTextAsync(
        x: Dimension.X.Value,
        y: Dimension.Y.Value + 50,
        text: "Strikethrough Text",
        fontFamily: "Arial",
        fontSize: 24,
        bold: false,
        italic: false,
        color: "red",
        isSelected: false,
        degree: 0,
        fillColor: "",
        strokeColor: "",
        strokeWidth: -1,
        underline: false,
        strikethrough: true          // Strikethrough
    );
    
    // Combined formatting
    await ImageEditor.DrawTextAsync(
        x: Dimension.X.Value,
        y: Dimension.Y.Value + 90,
        text: "Bold Underlined",
        fontFamily: "Arial",
        fontSize: 24,
        bold: true,
        italic: false,
        color: "blue",
        isSelected: false,
        degree: 0,
        fillColor: "",
        strokeColor: "",
        strokeWidth: -1,
        underline: true,
        strikethrough: false
    );
}
`

## Annotation Z-Order Management

Control the stacking order of annotations with z-order methods.

### Bring to Front

Move an annotation to the top of all other annotations:

`csharp
private async Task BringAnnotationToFront()
{
    ShapeSettings[] shapes = await ImageEditor.GetShapesAsync();
    
    if (shapes != null && shapes.Length > 0)
    {
        // Move first shape to front
        await ImageEditor.BringToFrontAsync(shapes[0].ID);
    }
}
`

### Send to Back

Move an annotation behind all other annotations:

`csharp
private async Task SendAnnotationToBack()
{
    ShapeSettings[] shapes = await ImageEditor.GetShapesAsync();
    
    if (shapes != null && shapes.Length > 0)
    {
        // Move first shape to back
        await ImageEditor.SendToBackAsync(shapes[0].ID);
    }
}
`

### Bring Forward

Move an annotation one layer forward:

`csharp
private async Task MoveAnnotationForward()
{
    ShapeSettings[] shapes = await ImageEditor.GetShapesAsync();
    
    if (shapes != null && shapes.Length > 0)
    {
        // Move one layer forward
        await ImageEditor.BringForwardAsync(shapes[0].ID);
    }
}
`

### Send Backward

Move an annotation one layer backward:

`csharp
private async Task MoveAnnotationBackward()
{
    ShapeSettings[] shapes = await ImageEditor.GetShapesAsync();
    
    if (shapes != null && shapes.Length > 0)
    {
        // Move one layer backward
        await ImageEditor.SendBackwardAsync(shapes[0].ID);
    }
}
`

### Z-Order Use Cases

**Layering text over shapes:**
`csharp
// 1. Draw background rectangle
await ImageEditor.DrawRectangleAsync(100, 100, 300, 150, 2, "black", "lightyellow");

// 2. Draw text
await ImageEditor.DrawTextAsync(150, 150, "Label", "Arial", 24);

// 3. If text is behind, bring it forward
ShapeSettings[] shapes = await ImageEditor.GetShapesAsync();
string textShapeId = shapes.FirstOrDefault(s => s.Type == ShapeType.Text)?.ID;
if (textShapeId != null)
{
    await ImageEditor.BringToFrontAsync(textShapeId);
}
`

**Organizing complex annotations:**
`csharp
// Move important annotations to front
foreach (var shape in importantShapes)
{
    await ImageEditor.BringToFrontAsync(shape.ID);
}

// Send background elements to back
foreach (var shape in backgroundShapes)
{
    await ImageEditor.SendToBackAsync(shape.ID);
}
`

## Clone Annotations

Duplicate existing annotations:

`csharp
private async Task DuplicateAnnotation()
{
    ShapeSettings[] shapes = await ImageEditor.GetShapesAsync();
    
    if (shapes != null && shapes.Length > 0)
    {
        // Clone first annotation
        bool success = await ImageEditor.CloneShapeAsync(shapes[0].ID);
        
        if (success)
        {
            Console.WriteLine("Annotation cloned successfully");
        }
    }
}
`

**Use Cases:**
- Duplicate watermarks across image
- Create pattern of similar annotations
- Quick copy of styled elements

## Enable/Disable Annotation Modes

Control annotation drawing modes programmatically:

### Enable Specific Annotation

`csharp
private async Task EnableRectangleDrawing()
{
    // Enable rectangle drawing mode
    await ImageEditor.EnableActiveAnnotationAsync(ShapeType.Rectangle);
    
    // User can now draw multiple rectangles
    // Drawing continues until disabled
}

private async Task EnableTextAnnotation()
{
    // Enable text annotation mode
    await ImageEditor.EnableActiveAnnotationAsync(ShapeType.Text);
}

private async Task EnableFreehandDrawing()
{
    // Enable freehand drawing
    await ImageEditor.EnableActiveAnnotationAsync(ShapeType.FreehandDraw);
}
`

### Disable Annotation Mode

`csharp
private async Task StopDrawing()
{
    // Disable current annotation mode
    await ImageEditor.DisableActiveAnnotationAsync();
    
    // User can no longer draw the current annotation type
}
`

### Available ShapeTypes for EnableActiveAnnotationAsync

- ShapeType.Rectangle
- ShapeType.Ellipse
- ShapeType.Line
- ShapeType.Arrow
- ShapeType.Path
- ShapeType.Text
- ShapeType.FreehandDraw
- ShapeType.Image

### Annotation Mode Workflow

`csharp
// Pattern: Toggle annotation mode
private bool isDrawingMode = false;

private async Task ToggleDrawingMode()
{
    if (isDrawingMode)
    {
        await ImageEditor.DisableActiveAnnotationAsync();
        isDrawingMode = false;
        Console.WriteLine("Drawing mode disabled");
    }
    else
    {
        await ImageEditor.EnableActiveAnnotationAsync(ShapeType.Rectangle);
        isDrawingMode = true;
        Console.WriteLine("Rectangle drawing enabled");
    }
}
`

## Enable Text Editing

Allow editing of text annotations:

`csharp
private async Task EnableTextEdit()
{
    // Enable text editing mode
    await ImageEditor.EnableTextEditingAsync();
    
    // User can now edit text annotations by clicking them
}
`

**Use Cases:**
- Modify existing text annotations
- Correct typos
- Update labels

All advanced annotation methods support undo/redo and are tracked in history.
