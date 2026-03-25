# Toolbar Customization in the Image Editor

## Table of Contents
- [Built-in Toolbar Items](#built-in-toolbar-items)
- [Custom Toolbar](#custom-toolbar)
- [Show and Hide Toolbar](#show-and-hide-toolbar)
- [Enable and Disable Items](#enable-and-disable-items)
- [Contextual Toolbar](#contextual-toolbar)
- [Toolbar Events](#toolbar-events)
- [Toolbar Template](#toolbar-template)

## Built-in Toolbar Items

### Default Toolbar Items

The Image Editor includes these built-in toolbar items by default:

| Item | Icon | Purpose |
|------|------|---------|
| **Open** | 📂 | Load image from file system |
| **Undo** | ↶ | Undo last operation |
| **Redo** | ↷ | Redo last undone operation |
| **ZoomIn** | 🔍+ | Zoom in to image |
| **ZoomOut** | 🔍- | Zoom out from image |
| **Crop** | ✂️ | Crop and transform images |
| **RotateLeft** | ⟲ | Rotate counter-clockwise 90° |
| **RotateRight** | ⟳ | Rotate clockwise 90° |
| **HorizontalFlip** | ↔️ | Mirror left-right |
| **VerticalFlip** | ↕️ | Mirror top-bottom |
| **Straightening** | ≈ | Straighten with slider |
| **Annotation** | 📝 | Add text, shapes, drawings |
| **Pen** | ✏️ | Freehand drawing tool |
| **Line** | ─ | Draw lines |
| **Ellipse** | ◯ | Draw ellipses |
| **Path** | ∿ | Draw paths |
| **Arrow** | → | Draw arrows |
| **Finetune** | 🎚️ | Apply adjustments |
| **Filter** | 🎨 | Apply filters |
| **Frame** | 🖼️ | Add frames |
| **Resize** | ⟷ | Resize image dimensions |
| **Redact** | ▓ | Redact sensitive areas |
| **Reset** | 🔄 | Reset to original image |
| **Save** | 💾 | Export/save image |

## Custom Toolbar

### Show Only Specific Items

Define which toolbar items to display:

```csharp
@using Syncfusion.Blazor.ImageEditor

<SfImageEditor @ref="ImageEditor" Toolbar="@CustomToolbar" Height="500px">
    <ImageEditorEvents Created="OnCreated"></ImageEditorEvents>
</SfImageEditor>

@code {
    SfImageEditor ImageEditor;

    private List<ImageEditorToolbarItemModel> CustomToolbar = new()
    {
        new ImageEditorToolbarItemModel { Name = "Open" },
        new ImageEditorToolbarItemModel { Name = "Crop" },
        new ImageEditorToolbarItemModel { Name = "Annotation" },
        new ImageEditorToolbarItemModel { Name = "Finetune" },
        new ImageEditorToolbarItemModel { Name = "Save" }
    };

    private async void OnCreated()
    {
        await ImageEditor.OpenAsync("https://example.com/image.png");
    }
}
```

### Add Custom Toolbar Items

Include custom buttons with text or icons:

```csharp
private List<ImageEditorToolbarItemModel> CustomToolbar = new()
{
    new ImageEditorToolbarItemModel { Name = "Open" },
    new ImageEditorToolbarItemModel { Name = "Crop" },
    new ImageEditorToolbarItemModel { Name = "Annotation" },
    // Custom item
    new ImageEditorToolbarItemModel 
    { 
        Text = "Rotate", 
        TooltipText = "Rotate 90°",
        Align = ItemAlign.Center 
    },
    new ImageEditorToolbarItemModel { Name = "Save" }
};
```

### Handle Custom Item Clicks

Respond to custom toolbar button clicks:

```csharp
<SfImageEditor @ref="ImageEditor" Toolbar="@CustomToolbar" Height="500px">
    <ImageEditorEvents 
        Created="OnCreated" 
        ToolbarItemClicked="OnToolbarItemClicked">
    </ImageEditorEvents>
</SfImageEditor>

@code {
    SfImageEditor ImageEditor;

    private async void OnToolbarItemClicked(ClickEventArgs args)
    {
        if (args.Item.Text == "Rotate")
        {
            await ImageEditor.RotateAsync(90);
        }
    }
}
```

## Show and Hide Toolbar

### Hide Entire Toolbar

Set an empty toolbar to hide it completely:

```csharp
<SfImageEditor @ref="ImageEditor" Toolbar="@CustomToolbar" Height="500px">
    <ImageEditorEvents Created="OnCreated"></ImageEditorEvents>
</SfImageEditor>

@code {
    // Empty list hides toolbar
    private List<ImageEditorToolbarItemModel> CustomToolbar = new();
}
```

**Result:** Image editor renders without toolbar, but component still functions.

### Show Toolbar Again

Populate the toolbar list to show it:

```csharp
private List<ImageEditorToolbarItemModel> CustomToolbar = new()
{
    new ImageEditorToolbarItemModel { Name = "Open" },
    new ImageEditorToolbarItemModel { Name = "Save" }
};
```

### Conditional Visibility

Show/hide toolbar based on state:

```csharp
@if (ShowToolbar)
{
    <SfImageEditor @ref="ImageEditor" Toolbar="@ToolbarItems" Height="500px">
    </SfImageEditor>
}

@code {
    private bool ShowToolbar = true;
    private List<ImageEditorToolbarItemModel> ToolbarItems;
}
```

## Enable and Disable Items

### Disable Specific Items

Prevent users from using certain tools:

```csharp
private List<ImageEditorToolbarItemModel> CustomToolbar = new()
{
    new ImageEditorToolbarItemModel { Name = "Open" },
    new ImageEditorToolbarItemModel { Name = "Crop" },
    new ImageEditorToolbarItemModel { Name = "Annotation", Disabled = true },
    new ImageEditorToolbarItemModel { Name = "Finetune", Disabled = true },
    new ImageEditorToolbarItemModel { Name = "Filter" },
    new ImageEditorToolbarItemModel { Name = "Save" }
};
```

Disabled items appear grayed out and cannot be clicked.

### Conditional Enable/Disable

Enable/disable items based on conditions:

```csharp
@using Syncfusion.Blazor.ImageEditor

<SfImageEditor @ref="ImageEditor" Toolbar="@UpdateToolbar()" Height="500px">
</SfImageEditor>

@code {
    SfImageEditor ImageEditor;
    private bool AllowAnnotations = false;

    private List<ImageEditorToolbarItemModel> UpdateToolbar()
    {
        return new()
        {
            new ImageEditorToolbarItemModel { Name = "Open" },
            new ImageEditorToolbarItemModel { Name = "Crop" },
            new ImageEditorToolbarItemModel 
            { 
                Name = "Annotation", 
                Disabled = !AllowAnnotations 
            },
            new ImageEditorToolbarItemModel { Name = "Save" }
        };
    }
}
```

## Contextual Toolbar

### ToolbarUpdating Event

Customize the contextual toolbar when annotations are selected or inserted:

```csharp
<SfImageEditor @ref="ImageEditor" Height="500px">
    <ImageEditorEvents 
        Created="OnCreated" 
        ToolbarUpdating="OnToolbarUpdating">
    </ImageEditorEvents>
</SfImageEditor>

@code {
    SfImageEditor ImageEditor;

    private void OnToolbarUpdating(ToolbarEventArgs args)
    {
        // args.ToolbarType: ShapeType of the annotation
        // args.ToolbarItems: List of contextual toolbar items
        
        if (args.ToolbarType == ShapeType.FreehandDraw)
        {
            // Show only stroke color for freehand
            args.ToolbarItems = new List<ImageEditorToolbarItemModel>()
            {
                new ImageEditorToolbarItemModel { Name = "StrokeColor" }
            };
        }
    }
}
```

### Contextual Toolbar Types

Each annotation type has contextual options:

| Type | Available Options |
|------|-------------------|
| **Text** | Font, FontSize, Color, Bold, Italic, Underline, Strikethrough |
| **Shape** | StrokeColor, StrokeWidth, FillColor |
| **FreehandDraw** | StrokeColor, StrokeWidth |
| **Arrow** | StrokeColor, StrokeWidth |

## Toolbar Events

### ToolbarCreated Event

Triggered after toolbar initialization:

```csharp
<SfImageEditor @ref="ImageEditor" Height="500px">
    <ImageEditorEvents Created="OnCreated" ToolbarCreated="OnToolbarCreated">
    </ImageEditorEvents>
</SfImageEditor>

@code {
    private void OnToolbarCreated()
    {
        // Toolbar is now ready
        // Perform any post-initialization tasks
    }
}
```

### ToolbarItemClicked Event

Triggered when user clicks a toolbar item:

```csharp
<SfImageEditor @ref="ImageEditor" Toolbar="@CustomToolbar" Height="500px">
    <ImageEditorEvents ToolbarItemClicked="OnToolbarItemClicked">
    </ImageEditorEvents>
</SfImageEditor>

@code {
    private async void OnToolbarItemClicked(ClickEventArgs args)
    {
        if (args.Item.Text == "CustomRotate")
        {
            await ImageEditor.RotateAsync(45);  // Custom angle
        }
    }
}
```

## Toolbar Template

### Custom Toolbar Template

Replace the entire toolbar with a custom template:

```csharp
@using Syncfusion.Blazor.ImageEditor
@using Syncfusion.Blazor.Buttons

<SfImageEditor @ref="ImageEditor" Height="400px">
    <ImageEditorTemplates>
        <ToolbarTemplate>
            <div class="e-toolbar" style="display: flex; gap: 10px; padding: 10px;">
                <SfButton OnClick="OpenImage">Open</SfButton>
                <SfButton OnClick="EnableFreehand">Freehand Draw</SfButton>
                <SfButton OnClick="SaveImage">Save</SfButton>
            </div>
        </ToolbarTemplate>
    </ImageEditorTemplates>
    <ImageEditorEvents Created="OnCreated"></ImageEditorEvents>
</SfImageEditor>

@code {
    SfImageEditor ImageEditor;

    private async void OnCreated()
    {
        await ImageEditor.OpenAsync("https://example.com/image.png");
    }

    private async void OpenImage()
    {
        // Open image logic
    }

    private async void EnableFreehand()
    {
        await ImageEditor.EnableFreehandDrawAsync();
    }

    private async void SaveImage()
    {
        // Export logic
    }
}
```

### Template Styling

Style custom toolbar:

```html
<style>
    .custom-toolbar {
        display: flex;
        justify-content: space-between;
        align-items: center;
        padding: 10px;
        background-color: #f5f5f5;
        border-bottom: 1px solid #ddd;
        gap: 5px;
    }

    .toolbar-group {
        display: flex;
        gap: 5px;
    }

    .toolbar-separator {
        width: 1px;
        background-color: #ccc;
        height: 24px;
    }
</style>
```

## Complete Working Example

### Full Blazor Component with Toolbar Presets

The component `ImageEditorToolbarCustomization.razor` demonstrates:
- Multiple toolbar presets (Minimal, Balanced, Full, Custom)
- Toggle toolbar visibility
- Conditional enable/disable of items
- Custom toolbar items with click handlers
- Status feedback

**Location:** `components/ImageEditor/ImageEditorToolbarCustomization.razor`

**Quick Start:**
```csharp
// In Program.cs
builder.Services.AddSyncfusionBlazor();

// In your component
@page "/image-editor-toolbar"
<ImageEditorToolbarCustomization />
```

**Features:**
- Dropdown to switch between 4 toolbar presets
- Checkbox to show/hide toolbar
- Checkbox to toggle annotations
- Real-time feedback with status messages
- Custom rotate and export buttons

Navigate to `/image-editor-toolbar` and experiment with different presets!

## Best Practices

### Toolbar Configuration

- **Minimal:** Show only essential tools (Open, Crop, Save)
- **Balanced:** Include common tools (Crop, Annotation, Filter, Save)
- **Full:** Enable all tools for power users
- **Custom:** Create domain-specific toolbars

### User Experience

- Group related items together
- Disable tools that don't apply to current image
- Provide keyboard shortcuts for frequently used items
- Use tooltips for clarity
- Keep toolbar items consistent with application design

### Accessibility

- Ensure disabled items have clear indication
- Provide alternative methods for toolbar actions
- Support keyboard navigation
- Include aria labels for screen readers

### Mobile Considerations

- Reduce toolbar items on small screens
- Use dropdown groups to save space
- Consider touch-friendly button sizes (minimum 44x44px)
- Test contextual toolbar on mobile devices
