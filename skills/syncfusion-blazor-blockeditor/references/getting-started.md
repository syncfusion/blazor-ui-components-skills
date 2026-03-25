# Getting Started with Syncfusion Blazor Block Editor

## Installation and Setup

### Prerequisites

- .NET 8.0 SDK or later
- Visual Studio 2022, Visual Studio Code, or .NET CLI
- Basic knowledge of Blazor and Razor components

### Step 1: Create a Blazor Web App

**Using Visual Studio:**
1. Create a new Blazor Web App project
2. Choose interactive render mode (Auto, WebAssembly, or Server)
3. Configure interactivity location as needed

**Using Visual Studio Code:**
```bash
dotnet new blazor -o MyBlockEditorApp -int Auto
cd MyBlockEditorApp
cd MyBlockEditorApp.Client
```

**Using .NET CLI:**
```bash
dotnet new blazor -o MyBlockEditorApp -int Auto
cd MyBlockEditorApp
cd MyBlockEditorApp.Client
```

### Step 2: Install NuGet Packages

**Using NuGet Package Manager (Visual Studio):**
1. Open NuGet Package Manager
2. Search for `Syncfusion.Blazor.BlockEditor`
3. Install both `Syncfusion.Blazor.BlockEditor` and `Syncfusion.Blazor.Themes`

**Using Package Manager Console:**
```powershell
Install-Package Syncfusion.Blazor.BlockEditor -Version 28.0.0
Install-Package Syncfusion.Blazor.Themes -Version 28.0.0
```

**Using .NET CLI:**
```bash
dotnet add package Syncfusion.Blazor.BlockEditor --version 28.0.0
dotnet add package Syncfusion.Blazor.Themes --version 28.0.0
dotnet restore
```

### Step 3: Register Syncfusion Service

Open `Program.cs` in your project and add the Syncfusion Blazor service:

**For Server-Side Rendering:**
```csharp
using Syncfusion.Blazor;

var builder = WebApplication.CreateBuilder(args);
builder.Services.AddRazorComponents()
    .AddInteractiveServerComponents()
    .AddInteractiveWebAssemblyComponents();
builder.Services.AddSyncfusionBlazor();

var app = builder.Build();
// ... rest of configuration
```

**For WebAssembly (Client project):**
```csharp
using Syncfusion.Blazor;

var builder = WebAssemblyHostBuilder.CreateDefault(args);
builder.Services.AddSyncfusionBlazor();

await builder.Build().RunAsync();
```

### Step 4: Add Import Namespaces

Open `_Imports.razor` and add:

```razor
@using Syncfusion.Blazor
@using Syncfusion.Blazor.BlockEditor
```

### Step 5: Include Theme Resources

In `App.razor`, add the theme stylesheet and script references:

```html
<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Block Editor App</title>
    <base href="/" />
    <link href="_framework/blazor.web.css" rel="stylesheet" />
    <!-- Add Syncfusion theme -->
    <link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
</head>
<body>
    <!-- ... existing content ... -->
    <!-- Add Syncfusion script at the end of body -->
    <script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js" type="text/javascript"></script>
</body>
```

**Available Themes:**
- `bootstrap5.css` - Bootstrap 5 theme
- `material.css` - Material Design theme
- `material-dark.css` - Material Dark theme
- `tailwind.css` - Tailwind CSS theme
- `tailwind-dark.css` - Tailwind Dark theme
- `fabric.css` - Fabric Design theme
- `fluent.css` - Fluent Design theme

Choose the theme that matches your application design.

## Basic Component Initialization

### Minimal Block Editor

Create a simple editor with default configuration:

```razor
@page "/block-editor"
@rendermode InteractiveAuto

<SfBlockEditor></SfBlockEditor>
```

### Block Editor with Initial Content

Bind blocks to create an editor with predefined content:

```razor
@page "/block-editor"
@rendermode InteractiveAuto

<SfBlockEditor @bind-Blocks="blockData"></SfBlockEditor>

@code {
    private List<BlockModel> blockData = new()
    {
        new BlockModel
        {
            BlockType = BlockType.Heading,
            Properties = new HeadingBlockSettings { Level = 1 },
            Content = new()
            {
                new ContentModel { ContentType = ContentType.Text, Content = "Welcome to Block Editor" }
            }
        },
        new BlockModel
        {
            BlockType = BlockType.Paragraph,
            Content = new()
            {
                new ContentModel { ContentType = ContentType.Text, Content = "This is a paragraph block with text content." }
            }
        },
        new BlockModel
        {
            BlockType = BlockType.Paragraph,
            Content = new() // Empty paragraph for user input
        }
    };
}
```

## Render Modes

Choose the appropriate render mode for your use case:

### Auto Render Mode
Renders on the server initially, then switches to WebAssembly:
```razor
@rendermode InteractiveAuto
```
**Best for:** Applications needing fast initial load with client-side interactivity

### WebAssembly Render Mode
Runs entirely in the browser:
```razor
@rendermode InteractiveWebAssembly
```
**Best for:** Progressive Web Apps, offline-first applications

### Server Render Mode
All processing happens on the server:
```razor
@rendermode InteractiveServer
```
**Best for:** Real-time collaboration, server-side state management

## Configuration Example

```razor
@page "/editor"
@rendermode InteractiveAuto

<div id="editor-container" style="display: flex; flex-direction: column; gap: 20px;">
    <div style="padding: 15px; background-color: #f5f5f5; border-radius: 4px;">
        <h2>Block Editor Configuration</h2>
        <p>Start editing below or press "/" to open the command menu:</p>
    </div>

    <SfBlockEditor @bind-Blocks="blockData"
                   Width="100%"
                   Height="500px"
                   EnableDragAndDrop="true"
                   UndoRedoStack="50">
    </SfBlockEditor>

    <div style="padding: 15px; background-color: #f0f7ff; border-radius: 4px;">
        <strong>Block Count:</strong> @blockData.Count
    </div>
</div>

@code {
    private List<BlockModel> blockData = new()
    {
        new BlockModel
        {
            BlockType = BlockType.Heading,
            Properties = new HeadingBlockSettings { Level = 1 },
            Content = new()
            {
                new ContentModel { ContentType = ContentType.Text, Content = "Structured Content Editor" }
            }
        },
        new BlockModel
        {
            BlockType = BlockType.Paragraph,
            Content = new()
            {
                new ContentModel { ContentType = ContentType.Text, Content = "This editor uses a block-based architecture for flexible, modular content creation." }
            }
        },
        new BlockModel
        {
            BlockType = BlockType.Heading,
            Properties = new HeadingBlockSettings { Level = 2 },
            Content = new()
            {
                new ContentModel { ContentType = ContentType.Text, Content = "Features" }
            }
        },
        new BlockModel
        {
            BlockType = BlockType.BulletList,
            Content = new()
            {
                new ContentModel { ContentType = ContentType.Text, Content = "Drag-and-drop block reordering" },
                new ContentModel { ContentType = ContentType.Text, Content = "Comprehensive keyboard shortcuts" },
                new ContentModel { ContentType = ContentType.Text, Content = "Rich text formatting options" },
                new ContentModel { ContentType = ContentType.Text, Content = "Media and table support" }
            }
        },
        new BlockModel
        {
            BlockType = BlockType.Paragraph,
            Content = new() // Empty for user input
        }
    };
}

<style>
    #editor-container {
        max-width: 900px;
        margin: 0 auto;
        padding: 20px;
    }
</style>
```

## Troubleshooting Installation

| Issue | Solution |
|-------|----------|
| NuGet packages not found | Ensure your NuGet source includes nuget.org; update package manager |
| Theme not applying | Verify theme CSS is linked in App.razor; check file path |
| Component not rendering | Ensure Syncfusion service is registered in Program.cs; verify namespaces imported |
| Events not firing | Confirm render mode is interactive (not Static); check event handler is defined |
| Styling not working | Clear browser cache; check browser console for CSS load errors |
