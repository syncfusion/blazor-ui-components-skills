---
name: syncfusion-blazor-image-editor
description: Implement comprehensive image editing capabilities in Blazor applications using the Syncfusion Image Editor component. Use this skill when implementing image editing, annotations, transformations, cropping, filtering, zooming, and panning features. Supports annotations (text, shapes, freehand), transformations (crop, rotate, flip, resize), effects (filters, fine-tuning), toolbar customization, and keyboard shortcuts.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
  category: "File Editors"
---

# Implementing the Syncfusion Blazor Image Editor Component

The Syncfusion Blazor Image Editor is a powerful, feature-rich component for building image editing capabilities into your Blazor applications. It provides a complete toolkit for image manipulation, annotation, transformation, and export with an intuitive UI and extensive API.

## Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Installation of NuGet packages (Syncfusion.Blazor.ImageEditor, Syncfusion.Blazor.Themes)
- Blazor WebAssembly and Web App setup
- Import namespaces and register services
- Add stylesheet and script references
- Render the basic component
- **NEW:** Component properties (AllowUndoRedo, IsReadOnly, Enabled, Theme, CssClass, EnableImageSmoothing)

### Core Operations
📄 **Read:** [references/core-operations.md](references/core-operations.md)
- Open/load images from file explorer
- **NEW:** Complete OpenAsync signature with all 6 parameters (resetChanges, fillColor, width, height, isAspectRatio)
- Save and export images (PNG, JPEG, SVG, WEBP formats)
- Image quality settings for JPEG export
- Undo and redo operations with keyboard shortcuts (CanUndoAsync, CanRedoAsync)
- Reset image to original state
- **NEW:** Apply and Discard changes, Clear image, Get image data methods

### Image Transformations
📄 **Read:** [references/image-transformations.md](references/image-transformations.md)
- Crop images with custom, circle, square, or ratio selections
- Rotate left, right, and arbitrary angles
- Flip horizontally and vertically
- Straighten images with slider control
- **NEW:** StraightenImageAsync method for precise angle adjustment (-45 to +45 degrees)
- Resize images to specific dimensions
- Pan images within the canvas

### Annotations
📄 **Read:** [references/annotations.md](references/annotations.md)
- Add text annotations with customization (font, size, color, bold, italic, underline, strikethrough)
- **NEW:** Complete DrawTextAsync with underline and strikethrough parameters
- Create multiline text annotations
- Enable freehand drawing with stroke control
- Draw shapes (rectangles, ellipses, arrows, paths, lines)
- Insert image annotations (watermarks, logos, decorative elements)
- **NEW:** Annotation z-order management (BringToFrontAsync, SendToBackAsync, BringForwardAsync, SendBackwardAsync)
- **NEW:** Clone annotations with CloneShapeAsync
- **NEW:** Enable/disable annotation modes (EnableActiveAnnotationAsync, DisableActiveAnnotationAsync)
- **NEW:** Enable text editing mode
- Delete and manage annotations using ShapeSettings and IDs

### Annotation Styling
📄 **Read:** [references/annotation-styling.md](references/annotation-styling.md)
- Customize stroke width, color, and fill colors
- Apply font styling for text annotations
- Use ShapeChanging event for dynamic customization
- Configure default stroke colors and styles
- Work with ShapeSettings properties

### Adjustments, Filters & Effects
📄 **Read:** [references/adjustments-filters-effects.md](references/adjustments-filters-effects.md)
- Apply fine-tuning adjustments (brightness, contrast, saturation, hue, blur, etc.)
- Use slider controls for adjustment preview
- Apply filters to images (predefined effects)
- Add frame effects to images
- Commit or preview changes before applying

### Redaction
📄 **Read:** [references/redaction.md](references/redaction.md) **NEW**
- Draw redactions with blur or pixelate effects
- RedactType enum (Blur, Pixelate)
- Control blur intensity and pixel size
- Get, select, update, and delete redactions
- RedactSettings class properties
- Privacy protection and GDPR compliance use cases

### Frames
📄 **Read:** [references/frames.md](references/frames.md) **NEW**
- Apply decorative frames to images
- FrameType enum (Mat, Bevel, Line, Inset, Hook)
- Customize frame colors, gradients, sizes
- FrameLineStyle for Line frames (Solid, Dashed, Dotted)
- Configure inset, offset, border radius, and line count
- Professional photo presentation examples

### Events Reference
📄 **Read:** [references/events-reference.md](references/events-reference.md) **NEW**
- Complete ImageEditorEvents documentation
- Lifecycle events (Created, Destroyed)
- File operation events (FileOpened, Saving)
- Transformation events (Cropping/Cropped, Rotating/Rotated, Flipping/Flipped)
- Shape events (ShapeChanging/ShapeChanged, resize/drag start/end)
- Adjustment events (ImageFiltering/Filtered, FinetuneValueChanging/Changed, FrameChanging/Changed)
- User interaction events (Zooming/Zoomed, OnPanStart/End, Clicked)
- Toolbar events (ToolbarUpdating, ToolbarItemClicked, QuickAccessToolbarOpening)
- History events (HistoryChanged)
- Event arguments reference for all events

### Toolbar Customization
📄 **Read:** [references/toolbar-customization.md](references/toolbar-customization.md)
- Reference of built-in toolbar items (Open, Crop, Rotate, Annotation, Filters, etc.)
- Add custom toolbar items and buttons
- Show/hide entire toolbar or specific items
- Enable/disable toolbar items conditionally
- Customize contextual toolbars using ToolbarUpdating event
- Create custom toolbar templates

### User Interactions
📄 **Read:** [references/user-interactions.md](references/user-interactions.md)
- Zoom methods (toolbar buttons, pinch gesture, mouse wheel, keyboard shortcuts)
- Pan/move images across the canvas
- Selection types for cropping
- Get image dimensions and coordinates
- Keyboard shortcuts reference (Ctrl+Z, Ctrl+Y, Ctrl+S, etc.)

### Accessibility & Localization
📄 **Read:** [references/accessibility-localization.md](references/accessibility-localization.md)
- WCAG compliance and accessibility features
- Keyboard navigation support
- Screen reader compatibility
- ARIA attributes
- Color contrast standards
- Localization and RTL support

### Setup Modes & Deployment
📄 **Read:** [references/setup-modes.md](references/setup-modes.md)
- WebAssembly app setup (Visual Studio, VS Code, .NET CLI)
- Web App setup with interactive render modes (Auto, WebAssembly, Server)
- Theme configuration (Bootstrap5, Fluent2, Material3, Tailwind3)
- Static Web Assets vs CDN references
- Script references and dependencies

## Quick Start Example

```csharp
@using Syncfusion.Blazor.ImageEditor

<SfImageEditor @ref="ImageEditor" Height="500px" Width="100%">
    <ImageEditorEvents Created="OnCreated"></ImageEditorEvents>
</SfImageEditor>

@code {
    SfImageEditor ImageEditor;

    private async void OnCreated()
    {
        // Load an image when component is ready
        await ImageEditor.OpenAsync("path/to/image.png");
    }
}
```

**Setup steps:**
1. Install NuGet packages: `Syncfusion.Blazor.ImageEditor` and `Syncfusion.Blazor.Themes`
2. Add imports in `_Imports.razor`: `@using Syncfusion.Blazor.ImageEditor`
3. Register service in `Program.cs`: `builder.Services.AddSyncfusionBlazor();`
4. Add theme stylesheet to `index.html` or `App.razor`
5. Add the component and load an image in the `Created` event

## Key Capabilities

| Feature | Purpose | Reference |
|---------|---------|-----------|
| **Image Loading** | Open JPEG, PNG, JPG, WEBP, BMP files | core-operations.md |
| **Text Annotations** | Add labels, captions, watermarks with underline/strikethrough | annotations.md |
| **Shape Annotations** | Draw rectangles, ellipses, arrows, paths with z-order control | annotations.md |
| **Freehand Drawing** | Sketch and draw directly on images | annotations.md |
| **Redaction** | Hide sensitive info with blur/pixelate for privacy compliance | redaction.md |
| **Frames** | Apply decorative borders (Mat, Bevel, Line, Inset, Hook) | frames.md |
| **Crop & Transform** | Crop with multiple selection types, rotate, flip, straighten | image-transformations.md |
| **Filters & Effects** | Apply fine-tuning and predefined filters | adjustments-filters-effects.md |
| **Zoom & Pan** | Multiple zoom methods and image panning | user-interactions.md |
| **Export** | Save as PNG, JPEG, SVG, WEBP with quality control | core-operations.md |
| **Undo/Redo** | Full history of operations | core-operations.md |
| **Events** | 30+ lifecycle, transformation, shape, and interaction events | events-reference.md |
| **Toolbar** | Built-in or custom toolbar with events | toolbar-customization.md |
| **Accessibility** | Keyboard navigation, screen readers, RTL, localization | accessibility-localization.md |

## Common Patterns

### Pattern 1: Load Image and Enable Annotations
```csharp
<SfImageEditor @ref="ImageEditor" Height="500px">
    <ImageEditorEvents Created="OnCreated"></ImageEditorEvents>
</SfImageEditor>

@code {
    SfImageEditor ImageEditor;
    
    private async void OnCreated()
    {
        await ImageEditor.OpenAsync("https://example.com/image.png");
    }
    
    private async Task AddTextAnnotation()
    {
        ImageDimension dim = await ImageEditor.GetImageDimensionAsync();
        await ImageEditor.DrawTextAsync(dim.X.Value, dim.Y.Value, "Important", "Arial", 24);
    }
}
```

### Pattern 2: Custom Toolbar
```csharp
<SfImageEditor @ref="ImageEditor" Toolbar="@CustomToolbar" Height="500px">
    <ImageEditorEvents Created="OnCreated" ToolbarItemClicked="OnToolbarClick"></ImageEditorEvents>
</SfImageEditor>

@code {
    private List<ImageEditorToolbarItemModel> CustomToolbar = new()
    {
        new ImageEditorToolbarItemModel { Name = "Open" },
        new ImageEditorToolbarItemModel { Name = "Crop" },
        new ImageEditorToolbarItemModel { Name = "Annotation" },
        new ImageEditorToolbarItemModel { Name = "Save" }
    };
}
```

### Pattern 3: Export Image with Quality Control
```csharp
private async Task ExportImage()
{
    // Export as PNG (lossless, default)
    await ImageEditor.ExportAsync("edited-image.png");
    
    // Export as JPEG with quality control (0.0 to 1.0)
    await ImageEditor.ExportAsync("photo.jpg", ImageEditorFileType.JPEG, 0.85);
    
    // Export as WEBP (modern format)
    await ImageEditor.ExportAsync("image.webp", ImageEditorFileType.WEBP);
    
    // Export as SVG (vector format)
    await ImageEditor.ExportAsync("graphic.svg", ImageEditorFileType.SVG);
    
    // Or press Ctrl+S to open export dialog with format selection
}
```

### Pattern 4: Privacy Redaction Workflow
```csharp
<SfImageEditor @ref="ImageEditor" Height="500px">
    <ImageEditorEvents Created="OnCreated"></ImageEditorEvents>
</SfImageEditor>

@code {
    SfImageEditor ImageEditor;
    
    private async Task RedactSensitiveInfo()
    {
        // Draw blur redaction over sensitive areas
        await ImageEditor.DrawRedactAsync(
            RedactType.Blur, 
            startX: 100, startY: 100,
            width: 200, height: 50,
            value: 30  // Blur intensity
        );
        
        // Draw pixelate redaction over other areas
        await ImageEditor.DrawRedactAsync(
            RedactType.Pixelate,
            startX: 400, startY: 200,
            width: 200, height: 50,
            value: 20  // Pixel size
        );
        
        // Export redacted image
        await ImageEditor.ExportAsync("redacted-document.png");
    }
}
```

### Pattern 5: Professional Photo Framing
```csharp
<SfImageEditor @ref="ImageEditor" Height="500px">
    <ImageEditorEvents Created="OnCreated"></ImageEditorEvents>
</SfImageEditor>

@code {
    SfImageEditor ImageEditor;
    
    private async Task ApplyPhotoFrame()
    {
        // Load portrait image
        await ImageEditor.OpenAsync("portrait.jpg");
        
        // Apply classic mat frame with gradient
        await ImageEditor.DrawFrameAsync(
            FrameType.Mat,
            color: "#FFFFFF",
            gradientColor: "#F5F5DC",
            size: 50,
            inset: 20
        );
        
        // Export framed image
        await ImageEditor.ExportAsync("framed-portrait.png");
    }
}
```

## Related Skills

- [Implementing Blazor Components](../implementing-syncfusion-blazor-components/) - Main library overview
- [Implementing Carousels](../navigation/implementing-carousels/) - Image carousel display
- [Implementing Rich Text Editor](../file-viewers-editors/implementing-rich-text-editor/) - Text editing capabilities

---

**Note:** All code examples use Blazor component syntax. Refer to the Getting Started guide for setup instructions specific to your Blazor hosting model (WebAssembly, Web App, Server).
