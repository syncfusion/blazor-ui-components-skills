# Core Operations in the Image Editor

## Table of Contents
- [Opening Images](#opening-images)
- [Saving and Exporting Images](#saving-and-exporting-images)
- [Undo and Redo Operations](#undo-and-redo-operations)
- [Reset Image](#reset-image)
- [Keyboard Shortcuts](#keyboard-shortcuts)

## Opening Images

### Using the Open Button

Users can open images through the toolbar's Open button:

1. Click the Open icon on the toolbar
2. File explorer displays JPEG, PNG, JPG, WEBP, and BMP files
3. Select an image to load it into the editor

Supported formats: JPEG, PNG, JPG, WEBP, BMP

### Programmatic Image Loading

Load images programmatically using the `OpenAsync` method with full control over image rendering.

**Complete Method Signature:**
```csharp
Task OpenAsync(
    object data,
    bool resetChanges = true,
    string fillColor = "",
    int width = -1,
    int height = -1,
    bool isAspectRatio = false
)
```

**Parameters:**
- **data** (object): URL string or image data URL to open
- **resetChanges** (bool): Reset all existing changes when opening (default: true)
- **fillColor** (string): Background color for transparent images (default: empty/transparent)
- **width** (int): Target width to render on canvas (default: -1 for original)
- **height** (int): Target height to render on canvas (default: -1 for original)
- **isAspectRatio** (bool): Maintain aspect ratio when scaling (default: false)

The component `ImageEditorProgrammaticLoading.razor` demonstrates 4 loading methods:

1. **From URL** — Load remote images (web URLs, blob storage URLs with SAS tokens)
2. **From File Input** — User uploads local files (converted to base64 data URLs)
3. **From Azure Blob Storage** — Load from Azure with SAS token (requires CORS)
4. **From Sample Images** — Preloaded demo images

**Quick Start:**
Navigate to `/image-editor-loading` to try all loading methods interactively.

#### Load from URL (Basic)

```csharp
@using Syncfusion.Blazor.ImageEditor

<SfImageEditor @ref="ImageEditor" Height="500px">
    <ImageEditorEvents Created="OnCreated"></ImageEditorEvents>
</SfImageEditor>

@code {
    SfImageEditor ImageEditor;

    private async void OnCreated()
    {
        // Simple load - resets all changes
        await ImageEditor.OpenAsync("YOUR_IMAGE_URL");
    }
}
```

#### Load with Advanced Options

```csharp
private async Task LoadImageWithOptions()
{
    // Load with specific dimensions and background
    await ImageEditor.OpenAsync(
        data: "YOUR_TRANSPARENT_LOGO_URL",
        resetChanges: false,        // Keep existing edits
        fillColor: "#FFFFFF",       // White background for transparency
        width: 800,                 // Target width
        height: 600,                // Target height
        isAspectRatio: true        // Maintain aspect ratio
    );
}

private async Task LoadWithoutReset()
{
    // Load new image without clearing undo history
    await ImageEditor.OpenAsync(
        "YOUR_IMAGE_URL",
        resetChanges: false
    );
}

private async Task LoadTransparentWithBackground()
{
    // Handle transparent PNGs with colored background
    await ImageEditor.OpenAsync(
        "YOUR_ICON_URL",
        resetChanges: true,
        fillColor: "#F0F0F0"  // Light gray background
    );
}
```

**Use Cases:**
- Remote image URLs (web URLs)
- Azure Blob Storage URLs with SAS tokens
- CDN-hosted images
- Any accessible HTTP(S) URL with proper CORS headers
- Loading transparent images with backgrounds
- Resizing images on load
- Preserving edit history across image loads

#### Load from File Input

```csharp
private async Task LoadFromFileAsync(InputFileChangeEventArgs e)
{
    try
    {
        var file = e.File;
        using var stream = file.OpenReadStream(maxAllowedSize: 5 * 1024 * 1024);
        using var ms = new MemoryStream();
        await stream.CopyToAsync(ms);
        
        // Convert to base64 data URL
        var base64 = Convert.ToBase64String(ms.ToArray());
        var dataUrl = $"data:{file.ContentType};base64,{base64}";
        
        await imageEditor.OpenAsync(dataUrl);
    }
    catch (Exception ex)
    {
        Console.WriteLine($"Error: {ex.Message}");
    }
}

// In markup:
// <input type="file" @onchange="LoadFromFileAsync" accept="image/*" />
```

**Key Points:**
- Accepts JPEG, PNG, WEBP, BMP files
- Converts file to base64 for in-memory editing
- Max file size: 5 MB (configurable)
- Works offline (no network required)
- File stays in browser memory; not uploaded unless exported

#### Load from Azure Blob Storage

```csharp
private async Task LoadFromBlobAsync()
{
    try
    {
        string blobUrl = "https://<account>.blob.core.windows.net/<container>/<blob>?<SAS_token>";
        await imageEditor.OpenAsync(blobUrl);
    }
    catch (Exception ex)
    {
        Console.WriteLine($"Error: {ex.Message}");
    }
}
```

**Requirements:**
- Valid Azure Blob Storage URL
- Include SAS token for authentication
- Enable CORS on blob container:
  ```
  Allowed origins: https://yourdomain.com
  Allowed methods: GET
  Max age: 3600
  ```
- If CORS unavailable, proxy the blob through your backend API

### Getting Image Dimensions

Retrieve the current image dimensions and coordinates:

```csharp
private async void GetImageInfo()
{
    ImageDimension Dimension = await ImageEditor.GetImageDimensionAsync();
    
    double x = Dimension.X.Value;      // X-coordinate
    double y = Dimension.Y.Value;      // Y-coordinate
    double width = Dimension.Width.Value;
    double height = Dimension.Height.Value;
}
```

Use this to position annotations or calculate layout relative to the image.

## Saving and Exporting Images

### Export using Toolbar Button

Click the Save button on the toolbar to export the modified image:

1. Save button opens the export dialog
2. Choose file format: PNG, JPEG, SVG, or WEBP
3. For JPEG, adjust quality (Good, Great, Highest, or 0-100 slider)
4. Click Download to save

### Quick Export with Keyboard Shortcut

Press `Ctrl+S` to export with minimal dialog interaction:

```csharp
// Keyboard shortcut handled automatically
// Ctrl+S downloads in the same format as the loaded image
```

### Programmatic Export with ExportAsync

Export images programmatically using the `ExportAsync` method with full control over format and quality.

**Method Signature:**
```csharp
Task ExportAsync(
    string fileName = "",
    ImageEditorFileType fileType = ImageEditorFileType.PNG,
    double imageQuality = 1
)
```

**Parameters:**
- **fileName** (string): Name of the exported file (optional, defaults to original name)
- **fileType** (ImageEditorFileType): Export format - PNG, JPEG, SVG, or WEBP (default: PNG)
- **imageQuality** (double): Quality for JPEG exports, 0.0 to 1.0 (default: 1.0)

**Basic Export Examples:**

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

    // Export as PNG (lossless, default)
    private async Task ExportAsPNG()
    {
        await ImageEditor.ExportAsync("edited-image.png");
        // or
        await ImageEditor.ExportAsync("edited-image.png", ImageEditorFileType.PNG);
    }

    // Export as JPEG with quality control
    private async Task ExportAsJPEG()
    {
        // High quality (100%)
        await ImageEditor.ExportAsync("photo.jpg", ImageEditorFileType.JPEG, 1.0);
        
        // Good quality (85%)
        await ImageEditor.ExportAsync("photo.jpg", ImageEditorFileType.JPEG, 0.85);
        
        // Medium quality (60%)
        await ImageEditor.ExportAsync("photo.jpg", ImageEditorFileType.JPEG, 0.6);
    }

    // Export as WEBP
    private async Task ExportAsWEBP()
    {
        await ImageEditor.ExportAsync("image.webp", ImageEditorFileType.WEBP);
    }

    // Export as SVG (vector format)
    private async Task ExportAsSVG()
    {
        await ImageEditor.ExportAsync("graphic.svg", ImageEditorFileType.SVG);
    }

    // Export with original filename (format only)
    private async Task ChangeFormatOnly()
    {
        // Keeps original filename, changes format
        await ImageEditor.ExportAsync("", ImageEditorFileType.JPEG, 0.9);
    }
}
```

**Use Cases:**
- Automated batch processing workflows
- Server-side image processing
- Custom export buttons with specific formats
- Quality optimization for web vs. print
- Format conversion without user interaction

### Export Formats

| Format | Quality | Use Case |
|--------|---------|----------|
| **PNG** | Lossless | Graphics, transparent backgrounds |
| **JPEG** | Adjustable (0-100) | Photos, compressed images |
| **SVG** | Vector | Scalable graphics, logos |
| **WEBP** | Compressed | Web optimization, modern browsers |

### JPEG Quality Settings

When exporting as JPEG:

- **Good:** ~60% quality
- **Great:** ~80% quality
- **Highest:** ~95% quality
- **Custom Slider:** 0-100 range

Higher quality retains more detail but increases file size.

## Undo and Redo Operations

### Undo Changes

Undo the last operation using the toolbar button or keyboard:

```csharp
@using Syncfusion.Blazor.ImageEditor

<SfImageEditor @ref="ImageEditor" Height="500px" AllowUndoRedo="true">
</SfImageEditor>

@code {
    SfImageEditor ImageEditor;

    private async Task UndoLastAction()
    {
        // Programmatic undo
        await ImageEditor.UndoAsync();
    }
    
    private async Task CheckUndoAvailability()
    {
        bool canUndo = await ImageEditor.CanUndoAsync();
        if (canUndo)
        {
            Console.WriteLine("Undo is available");
        }
    }
    
    // Keyboard: Ctrl+Z
    // Or click Undo button on toolbar
}
```

**Keyboard shortcut:** `Ctrl+Z`

### Redo Changes

Redo the last undone operation:

```csharp
private async Task RedoLastAction()
{
    // Programmatic redo
    await ImageEditor.RedoAsync();
}

private async Task CheckRedoAvailability()
{
    bool canRedo = await ImageEditor.CanRedoAsync();
    if (canRedo)
    {
        Console.WriteLine("Redo is available");
    }
}

// Keyboard: Ctrl+Y
// Or click Redo button on toolbar
```

**Keyboard shortcut:** `Ctrl+Y`

### Undo/Redo Behavior

- **Undo button** becomes enabled after the first operation
- **Redo button** becomes enabled after an undo action
- All operations are tracked: transformations, annotations, filters, adjustments
- Undo/redo stack persists for the session
- History is limited to 16 actions (oldest actions are removed after 16)

### AllowUndoRedo Property

Control undo/redo functionality:

```csharp
// Enable undo/redo (default)
<SfImageEditor AllowUndoRedo="true" Height="500px"></SfImageEditor>

// Disable undo/redo
<SfImageEditor AllowUndoRedo="false" Height="500px"></SfImageEditor>
```

**Note:** When `AllowUndoRedo` is false, undo/redo methods return without action and history is not tracked.

## Reset Image

### Reset to Original

Revert all modifications and return the image to its original state:

```csharp
@using Syncfusion.Blazor.ImageEditor

<SfImageEditor @ref="ImageEditor" Height="500px" Toolbar="@CustomToolbar">
</SfImageEditor>

@code {
    private List<ImageEditorToolbarItemModel> CustomToolbar = new()
    {
        new ImageEditorToolbarItemModel { Name = "Reset" }
    };
}
```

Click the Reset button on the toolbar to discard all changes. This action:
- Reverts all transformations (crop, rotate, flip)
- Removes all annotations
- Clears all filters and adjustments
- Returns the image to loaded state

**Note:** Reset is permanent for the current session. Changes are not undone step-by-step.

## Keyboard Shortcuts

### Navigation and Zoom

| Shortcut | Action |
|----------|--------|
| `Ctrl + +` | Zoom in |
| `Ctrl + −` | Zoom out |
| `Ctrl + 0` | Fit image to window |
| Arrow Keys | Pan image (when zoomed) |

### Editing Operations

| Shortcut | Action |
|----------|--------|
| `Ctrl + Z` | Undo last operation |
| `Ctrl + Y` | Redo last undone operation |
| `Ctrl + S` | Save/Export image |
| `Delete` | Delete selected annotation |

### Annotation

| Shortcut | Action |
|----------|--------|
| `Ctrl + Click` | Multi-select annotations |
| `Escape` | Deselect current annotation |
| `Enter` | Confirm annotation edit |

### Common Workflows

**Quick zoom and pan workflow:**
```
Ctrl+Scroll → Zoom
Drag → Pan
Ctrl+Z → Undo zoom
Ctrl+S → Save
```

**Annotation workflow:**
```
Click annotation tool
Draw annotation
Escape → Deselect
Delete → Remove
Ctrl+Z → Undo drawing
```

All keyboard shortcuts are non-blocking and work alongside mouse interactions.

## Additional Core Methods

### Apply and Discard Changes

Control when edits are committed:

`csharp
private async Task CommitAnnotations()
{
    // Apply current annotation drawings
    await ImageEditor.ApplyAsync();
}

private async Task CancelAnnotations()
{
    // Discard current annotation drawings
    await ImageEditor.DiscardAsync();
}
`

**Use Cases:**
- Commit freehand drawings
- Finalize annotation edits
- Cancel in-progress annotations

### Clear Image

Remove the loaded image and reset the editor:

`csharp
private async Task ClearEditor()
{
    // Clear loaded image
    await ImageEditor.ClearImageAsync();
    
    // Editor returns to initial empty state
}
`

### Refresh Component

Force re-render of the Image Editor:

`csharp
private async Task ForceRefresh()
{
    // Re-render the component
    await ImageEditor.RefreshAsync();
}
`

**Use Cases:**
- Fix rendering issues
- Update after external state changes
- Synchronize after dynamic updates

### Get Image Data

Retrieve image data for storage or processing:

`csharp
private async Task GetImageForStorage()
{
    // Get as byte array
    byte[] imageData = await ImageEditor.GetImageDataAsync();
    
    // Save to file system or database
    await SaveToDatabase(imageData);
}

private async Task GetImageDataUrl()
{
    // Get as data URL string
    string dataUrl = await ImageEditor.GetImageDataUrlAsync();
    
    // Can include or exclude annotations
    string withAnnotations = await ImageEditor.GetImageDataUrlAsync(includeAnnotations: true);
    string withoutAnnotations = await ImageEditor.GetImageDataUrlAsync(includeAnnotations: false);
    
    // Save to database or display in <img> tag
}
`

**GetImageDataAsync() Returns:**
- Byte array suitable for file I/O
- Use to render in canvas
- Can be passed back to `OpenAsync()`

**GetImageDataUrlAsync() Returns:**
- Base64-encoded data URL string
- Ready for `<img src="">` display
- Can be saved to database
- `includeAnnotations` parameter controls whether annotations are included
