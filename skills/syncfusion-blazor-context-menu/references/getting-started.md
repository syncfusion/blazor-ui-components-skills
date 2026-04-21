# Getting Started with Blazor ContextMenu

## Table of Contents
- [NuGet Installation](#nuget-installation)
- [Namespace Imports](#namespace-imports)
- [Service Registration](#service-registration)
- [Add Resources](#add-resources)
- [Basic Component](#basic-component)
- [Test Your Setup](#test-your-setup)

## NuGet Installation

Install the required Syncfusion packages via NuGet Package Manager or CLI:

**Package Manager Console:**
```
Install-Package Syncfusion.Blazor.Navigations
Install-Package Syncfusion.Blazor.Themes
```

**Or use .NET CLI:**
```powershell
dotnet add package Syncfusion.Blazor.Navigations
dotnet add package Syncfusion.Blazor.Themes
dotnet restore
```

**Required Packages:**
- `Syncfusion.Blazor.Navigations` - Contains ContextMenu component
- `Syncfusion.Blazor.Themes` - Contains theme stylesheets

## Namespace Imports

Open `_Imports.razor` and add these namespaces:

```csharp
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Navigations
```

This makes `SfContextMenu` and `MenuItem` available throughout your project.

## Service Registration

In `Program.cs`, register the Syncfusion Blazor service:

```csharp
using Syncfusion.Blazor;

var builder = WebAssemblyHostBuilder.CreateDefault(args);
builder.RootComponents.Add<App>("#app");
builder.RootComponents.Add<HeadOutlet>("head::after");

builder.Services.AddScoped(sp => new HttpClient 
    { BaseAddress = new Uri(builder.HostEnvironment.BaseAddress) });

// Add this line:
builder.Services.AddSyncfusionBlazor();

await builder.Build().RunAsync();
```

## Add Resources

In `index.html` (in the `<head>` section), add the theme stylesheet and script:

```html
<head>
    <!-- Other meta tags -->
    <link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" 
          rel="stylesheet" />
    <script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js" 
            type="text/javascript"></script>
</head>
```

**Available themes:**
- `bootstrap5.css` (default, modern)
- `material.css` (Material Design)
- `fluent.css` (Microsoft Fluent)
- `tailwind.css` (Tailwind styling)

## Basic Component

Add the ContextMenu to your Razor component (e.g., `Pages/Index.razor`):

```cshtml
@using Syncfusion.Blazor.Navigations

<div id="target">Right click/Touch hold to open the ContextMenu</div>

<SfContextMenu Target="#target" TValue="MenuItem">
    <MenuItems>
        <MenuItem Text="Cut"></MenuItem>
        <MenuItem Text="Copy"></MenuItem>
        <MenuItem Text="Paste"></MenuItem>
    </MenuItems>
</SfContextMenu>

<style>
    #target {
        border: 1px dashed; height: 150px; padding: 10px;
        position: relative; text-align: center; color: gray;
        user-select: none; font-size: 14px;
    }
</style>
```

**Key Elements:**
- `Target="#target"` - CSS selector of element to right-click
- `TValue="MenuItem"` - Generic type (usually MenuItem)
- `<MenuItems>` - Container for menu items
- `<MenuItem Text="...">` - Individual menu item

## Test Your Setup

1. Run your Blazor app: `dotnet watch run` or press `Ctrl+F5`
2. Navigate to the component page (e.g., `https://localhost:7000/`)
3. **Right-click** the gray box labeled "Right click/Touch hold..."
4. Verify the context menu appears with Cut, Copy, Paste options
5. Click any item to close the menu

**Expected Result:** Gray area responds to right-click with popup menu.

---

## Common Issues

**Issue:** Menu doesn't appear on right-click
- **Fix:** Verify CSS selector in `Target` matches HTML element ID
- **Example:** `Target="#target"` requires `<div id="target">`

**Issue:** Styles not loading (unstyled menu)
- **Fix:** Ensure stylesheet is added in `index.html` `<head>`
- **Fix:** Verify theme file path: `_content/Syncfusion.Blazor.Themes/bootstrap5.css`

**Issue:** MenuItem is not recognized
- **Fix:** Verify `@using Syncfusion.Blazor.Navigations` in `_Imports.razor`
- **Fix:** Verify `AddSyncfusionBlazor()` in `Program.cs`

---

## Next Steps

- **Customize menu items:** Add icons, custom templates, nested menus (see customization-and-nesting.md)
- **Add interactivity:** Bind events to handle menu clicks (see events-and-interactions.md)
- **Data-driven menus:** Populate from collections or databases (see data-binding.md)
