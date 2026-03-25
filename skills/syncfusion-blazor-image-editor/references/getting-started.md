# Getting Started with the Blazor Image Editor

## Overview

The Syncfusion Blazor Image Editor is a powerful component for image editing and manipulation. This guide covers installation, setup, and basic component rendering in Blazor applications.

## Prerequisites

- .NET 6.0 or later
- Visual Studio 2022, Visual Studio Code, or .NET CLI
- Basic knowledge of Blazor component development

## Installation

### Step 1: Install NuGet Packages

Install the required NuGet packages using NuGet Package Manager or the Package Manager Console:

```bash
Install-Package Syncfusion.Blazor.ImageEditor -Version [latest-version]
Install-Package Syncfusion.Blazor.Themes -Version [latest-version]
```

Or using .NET CLI:

```bash
dotnet add package Syncfusion.Blazor.ImageEditor
dotnet add package Syncfusion.Blazor.Themes
dotnet restore
```

The `Syncfusion.Blazor.ImageEditor` package provides the component, and `Syncfusion.Blazor.Themes` provides styling.

## Setup in Blazor WebAssembly App

### Step 2: Add Imports

Open the `_Imports.razor` file and add the following namespaces:

```razor
@using Syncfusion.Blazor
@using Syncfusion.Blazor.ImageEditor
```

This makes the ImageEditor component and related types available throughout your application.

### Step 3: Register Syncfusion Service

In `Program.cs`, register the Syncfusion Blazor service:

```csharp
using Syncfusion.Blazor;

var builder = WebAssemblyHostBuilder.CreateDefault(args);
builder.RootComponents.Add<App>("#app");
builder.RootComponents.Add<HeadOutlet>("head::after");

builder.Services.AddScoped(sp => new HttpClient 
{ 
    BaseAddress = new Uri(builder.HostEnvironment.BaseAddress) 
});

builder.Services.AddSyncfusionBlazor(); // Add this line

await builder.Build().RunAsync();
```

### Step 4: Add Stylesheet and Script

In `index.html`, add the theme stylesheet and script references to the `<head>` section:

```html
<head>
    ...
    <link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
    <script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js" 
            type="text/javascript"></script>
</head>
```

Available themes: `bootstrap5.css`, `fluent2.css`, `material3.css`, `tailwind3.css`

## Setup in Blazor Web App

### Step 2: Add Imports

In the `_Imports.razor` file (from the client project if using WebAssembly/Auto render modes):

```razor
@using Syncfusion.Blazor
@using Syncfusion.Blazor.ImageEditor
```

### Step 3: Register Syncfusion Service

Register the service in both the server and client `Program.cs` files:

**Server Program.cs:**
```csharp
using Syncfusion.Blazor;

var builder = WebApplication.CreateBuilder(args);

builder.Services.AddRazorComponents()
    .AddInteractiveServerComponents()
    .AddInteractiveWebAssemblyComponents();

builder.Services.AddSyncfusionBlazor(); // Add this line

var app = builder.Build();
...
```

**Client Program.cs (if using WebAssembly/Auto):**
```csharp
using Syncfusion.Blazor;

var builder = WebAssemblyHostBuilder.CreateDefault(args);
builder.Services.AddSyncfusionBlazor();

await builder.Build().RunAsync();
```

### Step 4: Add Stylesheet and Script

In `App.razor`, add references to the theme and script:

```html
<head>
    ...
    <link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
</head>

<body>
    ...
    <script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js" 
            type="text/javascript"></script>
</body>
```

## Render the Component

Create a new Blazor page (e.g., `Pages/ImageEditor.razor`) and add the component:

```razor
@page "/image-editor"

<SfImageEditor Height="500px"></SfImageEditor>
```

Run your application with `dotnet run` or press Ctrl+F5 in Visual Studio. The Image Editor component will render with an empty canvas.

## Basic Component with Image Loading

```razor
@page "/image-editor"
@using Syncfusion.Blazor.ImageEditor

<SfImageEditor @ref="ImageEditor" Height="500px" Width="100%">
    <ImageEditorEvents Created="OnCreated"></ImageEditorEvents>
</SfImageEditor>

@code {
    SfImageEditor ImageEditor;

    private async void OnCreated()
    {
        // Load an image when component is initialized
        await ImageEditor.OpenAsync("https://example.com/image.png");
    }
}
```

The `Created` event fires after the component is initialized, making it the ideal place to load images or configure initial settings.

## Available Themes

Select the appropriate theme stylesheet based on your design requirements:

- **Bootstrap5:** `bootstrap5.css` - Bootstrap-based styling
- **Fluent2:** `fluent2.css` - Microsoft Fluent design
- **Material3:** `material3.css` - Google Material Design 3
- **Tailwind3:** `tailwind3.css` - Tailwind CSS styling

## Next Steps

After setup:
1. Load images using the file explorer or `OpenAsync` method
2. Add annotations or transformations
3. Export modified images
4. Customize the toolbar as needed

Refer to other reference guides for detailed feature implementation.

## Component Properties

### Essential Properties

Configure the Image Editor behavior with these important properties:

#### AllowUndoRedo

Enable or disable undo/redo functionality:

`csharp
// Enable undo/redo (default)
<SfImageEditor AllowUndoRedo="true" Height="500px"></SfImageEditor>

// Disable undo/redo
<SfImageEditor AllowUndoRedo="false" Height="500px"></SfImageEditor>
`

**Default:** 	rue  
**Type:** ool  
**Note:** Undo/redo history is limited to 16 actions.

#### IsReadOnly

Make the Image Editor read-only (view-only mode):

`csharp
<SfImageEditor IsReadOnly="true" Height="500px">
    <ImageEditorEvents Created="OnCreated"></ImageEditorEvents>
</SfImageEditor>

@code {
    private async void OnCreated()
    {
        await ImageEditor.OpenAsync("https://example.com/image.png");
        // User can view but not edit
    }
}
`

**Default:** alse  
**Type:** ool  
**Use Cases:**
- Display-only mode
- Preview before editing
- Restricted user access

#### Enabled

Enable or disable user interactions:

`csharp
<SfImageEditor Enabled="@isEditorEnabled" Height="500px"></SfImageEditor>

@code {
    private bool isEditorEnabled = true;
    
    private void ToggleEditor()
    {
        isEditorEnabled = !isEditorEnabled;
    }
}
`

**Default:** 	rue  
**Type:** ool  
**Difference from IsReadOnly:** When disabled, all interactions are blocked including viewing controls.

#### ShowQuickAccessToolbar

Control the quick access toolbar visibility (shown when annotations are selected):

`csharp
// Show quick access toolbar (default)
<SfImageEditor ShowQuickAccessToolbar="true" Height="500px"></SfImageEditor>

// Hide quick access toolbar
<SfImageEditor ShowQuickAccessToolbar="false" Height="500px"></SfImageEditor>
`

**Default:** 	rue  
**Type:** ool  
**Quick Access Toolbar Provides:** Clone, Delete, Edit text options when annotations are selected.

#### Theme

Set the visual theme for selection UI elements:

`csharp
<SfImageEditor Theme="Theme.Bootstrap5" Height="500px"></SfImageEditor>

<SfImageEditor Theme="Theme.Material3" Height="500px"></SfImageEditor>

<SfImageEditor Theme="Theme.FluentDark" Height="500px"></SfImageEditor>
`

**Default:** Theme.Bootstrap5  
**Type:** Theme enum

**Available Themes:**
- Theme.Bootstrap5
- Theme.Bootstrap5Dark
- Theme.Tailwind
- Theme.TailwindDark
- Theme.Fluent
- Theme.FluentDark
- Theme.Material
- Theme.MaterialDark
- Theme.Material3
- Theme.Material3Dark
- Theme.HighContrast
- Theme.Bootstrap4
- Theme.Bootstrap
- Theme.BootstrapDark
- Theme.Fabric
- Theme.FabricDark

**Note:** Theme controls the selection UI, resize handles, and control colors. Main toolbar and canvas styling come from the referenced CSS theme file.

#### CssClass

Apply custom CSS classes for styling:

`csharp
<SfImageEditor CssClass="custom-editor dark-mode" Height="500px"></SfImageEditor>

<style>
    .custom-editor {
        border: 2px solid #3498db;
        border-radius: 8px;
    }
    
    .dark-mode {
        background-color: #2c3e50;
    }
</style>
`

**Default:** Empty string  
**Type:** string

#### EnableImageSmoothing

Control image smoothing (anti-aliasing) during rendering:

`csharp
// Disable smoothing for crisp pixels (default)
<SfImageEditor EnableImageSmoothing="false" Height="500px"></SfImageEditor>

// Enable smoothing for smoother scaled images
<SfImageEditor EnableImageSmoothing="true" Height="500px"></SfImageEditor>
`

**Default:** alse  
**Type:** ool  
**Use Cases:**
- **False:** Pixel art, text, sharp edges
- **True:** Photographs, smooth scaling, reduced pixelation

This controls the HTML5 Canvas imageSmoothingEnabled property.

### Complete Component Example

`csharp
@page "/advanced-editor"
@using Syncfusion.Blazor.ImageEditor

<SfImageEditor @ref="ImageEditor"
               Height="600px"
               Width="100%"
               AllowUndoRedo="true"
               IsReadOnly="false"
               Enabled="true"
               ShowQuickAccessToolbar="true"
               Theme="Theme.Material3"
               CssClass="professional-editor"
               EnableImageSmoothing="false"
               Toolbar="@CustomToolbar">
    <ImageEditorEvents Created="OnCreated"></ImageEditorEvents>
</SfImageEditor>

<style>
    .professional-editor {
        box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
        border-radius: 8px;
    }
</style>

@code {
    SfImageEditor ImageEditor;
    
    private List<ImageEditorToolbarItemModel> CustomToolbar = new()
    {
        new ImageEditorToolbarItemModel { Name = "Open" },
        new ImageEditorToolbarItemModel { Name = "Crop" },
        new ImageEditorToolbarItemModel { Name = "Annotation" },
        new ImageEditorToolbarItemModel { Name = "Finetune" },
        new ImageEditorToolbarItemModel { Name = "Filter" },
        new ImageEditorToolbarItemModel { Name = "Frame" },
        new ImageEditorToolbarItemModel { Name = "Save" }
    };

    private async void OnCreated()
    {
        await ImageEditor.OpenAsync("https://example.com/image.png");
    }
}
`

## Settings Classes

### FinetuneSettings

Configure finetune adjustment ranges (not directly set but defines available ranges):

Finetune adjustments: Brightness, Contrast, Saturation, Hue, Blur, Shadow, Exposure, Opacity

### ZoomSettings

Configure zoom behavior (controlled via ImageEditorZoomSettings class):

- Minimum zoom level
- Maximum zoom level  
- Zoom factor increments

### UploadSettings

Configure image upload behavior (controlled via ImageEditorUploadSettings class):

- Allowed file types
- Maximum file size
- Upload URL

### SelectionSettings

Configure selection behavior for cropping (controlled via ImageEditorSelectionSettings class):

- Selection types
- Aspect ratios
- Default selection size

**Note:** These settings classes are typically configured through component properties or methods rather than directly instantiated.
