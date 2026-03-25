# Getting Started with Blazor Ribbon

## Installation and Setup

This guide walks you through installing and configuring the Syncfusion Blazor Ribbon component in your Blazor project.

### Step 1: Install NuGet Packages

Install the required Syncfusion packages:

**Using Package Manager:**
```
Install-Package Syncfusion.Blazor.Ribbon
Install-Package Syncfusion.Blazor.Themes
```

**Using .NET CLI:**
```bash
dotnet add package Syncfusion.Blazor.Ribbon
dotnet add package Syncfusion.Blazor.Themes
dotnet restore
```

### Step 2: Import Namespaces

Add these imports to your `~/_Imports.razor` file:

```csharp
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Ribbon
@using Syncfusion.Blazor.SplitButtons  // If using split buttons or dropdowns
```

### Step 3: Register Syncfusion Service

Register the Syncfusion Blazor service in `Program.cs`:

```csharp
using Syncfusion.Blazor;

var builder = WebAssemblyHostBuilder.CreateDefault(args);
builder.RootComponents.Add<App>("#app");

// Register Syncfusion Blazor service
builder.Services.AddSyncfusionBlazor();

await builder.Build().RunAsync();
```

**For Blazor Server:**
```csharp
services.AddSyncfusionBlazor();
```

### Step 4: Add Theme Stylesheet

Add the theme CSS reference to your `index.html` or `App.razor`:

**In `index.html` (WebAssembly/Web App):**
```html
<head>
    <link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
</head>
<body>
    <div id="app"></div>
    <script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js"></script>
</body>
```

**In `App.razor` (Blazor Server):**
```html
<link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
<script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js"></script>
```

**Available Themes:**
- `bootstrap5.css` - Bootstrap 5 (recommended)
- `material.css` - Material Design
- `fluent.css` - Fluent UI (Microsoft)
- `tailwind.css` - Tailwind CSS
- `fabric.css` - Office Fabric

### Step 5: Create Your First Ribbon

Add a basic ribbon to a page (`Pages/Index.razor`):

```razor
@page "/"
@using Syncfusion.Blazor.Ribbon
@using Syncfusion.Blazor.SplitButtons

<div style="width: 100%">
    <SfRibbon>
        <RibbonTabs>
            <RibbonTab HeaderText="Home">
                <RibbonGroups>
                    <RibbonGroup HeaderText="Clipboard" Orientation="Orientation.Row">
                        <RibbonCollections>
                            <RibbonCollection>
                                <RibbonItems>
                                    <RibbonItem Type="RibbonItemType.Button">
                                        <RibbonButtonSettings Content="Cut" IconCss="e-icons e-cut">
                                        </RibbonButtonSettings>
                                    </RibbonItem>
                                </RibbonItems>
                            </RibbonCollection>
                        </RibbonCollections>
                    </RibbonGroup>
                </RibbonGroups>
            </RibbonTab>
        </RibbonTabs>
    </SfRibbon>
</div>
```

### Step 6: Run Your Application

**WebAssembly:**
```bash
dotnet run
```

**Server:**
```bash
dotnet run
```

Navigate to `https://localhost:5001` (or your configured URL). You should see the ribbon with a Home tab and Clipboard group.

## Minimal Working Example

Complete minimal example with all necessary setup:

```razor
@page "/ribbon-demo"
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Ribbon
@using Syncfusion.Blazor.SplitButtons

<h2>My First Ribbon</h2>

<SfRibbon>
    <RibbonTabs>
        <RibbonTab HeaderText="Home">
            <RibbonGroups>
                <RibbonGroup HeaderText="Editing">
                    <RibbonCollections>
                        <RibbonCollection>
                            <RibbonItems>
                                <RibbonItem Type="RibbonItemType.Button">
                                    <RibbonButtonSettings Content="Save" IconCss="e-icons e-save">
                                    </RibbonButtonSettings>
                                </RibbonItem>
                            </RibbonItems>
                        </RibbonCollection>
                    </RibbonCollections>
                </RibbonGroup>
            </RibbonGroups>
        </RibbonTab>
    </RibbonTabs>
</SfRibbon>
```

## Adding Icons

Use Syncfusion's built-in icon library (`e-icons` class) with common icon names:

```razor
<RibbonItem Type="RibbonItemType.Button">
    <RibbonButtonSettings Content="Save" IconCss="e-icons e-save"></RibbonButtonSettings>
</RibbonItem>
```

**Common Icons:**
- `e-save` - Save
- `e-cut` - Cut
- `e-copy` - Copy
- `e-paste` - Paste
- `e-undo` - Undo
- `e-redo` - Redo
- `e-print` - Print
- `e-bold` - Bold
- `e-italic` - Italic
- `e-underline` - Underline

For full icon list, see Syncfusion icon documentation.

## Project Types

### Blazor WebAssembly
```csharp
// Program.cs
var builder = WebAssemblyHostBuilder.CreateDefault(args);
builder.Services.AddSyncfusionBlazor();
```

### Blazor Server
```csharp
// Startup.cs or Program.cs (depends on .NET version)
services.AddSyncfusionBlazor();
```

### Blazor Web App (.NET 8+)
```csharp
// Program.cs
builder.Services.AddSyncfusionBlazor();
```

Add `@rendermode InteractiveServer` or `@rendermode InteractiveWebAssembly` to your page.

## Troubleshooting

**Issue: "SfRibbon is not recognized"**
- Solution: Add `@using Syncfusion.Blazor.Ribbon` to `_Imports.razor`

**Issue: Ribbon appears unstyled**
- Solution: Verify theme CSS link in `index.html` or `App.razor`
- Check: CDN availability or switch to different theme

**Issue: Icons not displaying**
- Solution: Ensure `syncfusion-blazor.min.js` script is loaded
- Check: Icon CSS classes are correct (e.g., `e-icons e-save`)

**Issue: Component not rendering**
- Solution: Verify service is registered in `Program.cs`
- Confirm: Project has correct .NET version (6.0+)

## Render Modes (Blazor Web App)

If using Blazor Web App (.NET 8+), specify render mode on the component:

```razor
@rendermode InteractiveServer
```

or

```razor
@rendermode InteractiveWebAssembly
```

## License Configuration

If using trial or commercial license, register in `Program.cs`:

```csharp
using Syncfusion.Licensing;

// Add before calling builder.Build()
SyncfusionLicenseProvider.RegisterLicense("YOUR_LICENSE_KEY");

var host = builder.Build();
```

Get a free community license: https://www.syncfusion.com/sales/products

## Next Steps

1. Learn about **Ribbon Structure** (tabs, groups, collections, items)
2. Explore **Different Item Types** (buttons, dropdowns, checkboxes, etc.)
3. Add **File Menu** for file operations
4. Handle **Events** for user interactions
5. Apply **Theming** for consistent styling

For more examples and demos, visit: https://blazor.syncfusion.com/demos/ribbon/
