# Getting Started with Blazor Menu Bar

## Table of Contents
- [System Requirements](#system-requirements)
- [Creating a Blazor App](#creating-a-blazor-app)
- [Installing NuGet Packages](#installing-nuget-packages)
- [Configuring Imports and Services](#configuring-imports-and-services)
- [Adding Theme Resources](#adding-theme-resources)
- [Creating Your First Menu](#creating-your-first-menu)

## System Requirements

Before you begin, ensure your system meets these requirements:
- Visual Studio 2022 or later
- Visual Studio Code with .NET extensions
- .NET SDK 7.0 or later
- A modern web browser

Refer to official [System requirements for Blazor components](https://blazor.syncfusion.com/documentation/system-requirements) for complete details.

## Creating a Blazor App

### Using Visual Studio

1. Open Visual Studio 2022
2. Click **Create a new project**
3. Select **Blazor WebAssembly App** template
4. Configure project name and location
5. Choose ASP.NET Core Hosted or Standalone

Alternatively, use the **Syncfusion Blazor Extension** for Visual Studio to create a project with pre-configured Syncfusion packages.

### Using Visual Studio Code

Create a Blazor WebAssembly App with the following terminal command:

```bash
dotnet new blazorwasm -o BlazorMenuBarApp
cd BlazorMenuBarApp
```

### Using .NET CLI

```bash
dotnet new blazorwasm -o BlazorMenuBarApp
cd BlazorMenuBarApp
```

## Installing NuGet Packages

You need two essential packages for Menu Bar functionality:

### Via Package Manager Console (Visual Studio)

```powershell
Install-Package Syncfusion.Blazor.Navigations
Install-Package Syncfusion.Blazor.Themes
```

### Via .NET CLI

```bash
dotnet add package Syncfusion.Blazor.Navigations
dotnet add package Syncfusion.Blazor.Themes
dotnet restore
```

### Package Details

- **Syncfusion.Blazor.Navigations**: Contains Menu Bar, Accordion, Tabs, Sidebar, and other navigation components
- **Syncfusion.Blazor.Themes**: Provides CSS theme files (bootstrap5, material, fabric, tailwind, etc.)

> All Syncfusion Blazor packages are available on [nuget.org](https://www.nuget.org/packages?q=syncfusion.blazor)

## Configuring Imports and Services

### Step 1: Update _Imports.razor

Open `~/_Imports.razor` and add the required namespaces:

```razor
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Navigations
```

### Step 2: Register Syncfusion Service

Open `~/Program.cs` and register the Syncfusion Blazor service:

```csharp
using Syncfusion.Blazor;

var builder = WebAssemblyHostBuilder.CreateDefault(args);
builder.RootComponents.Add<App>("#app");
builder.RootComponents.Add<HeadOutlet>("head::after");

builder.Services.AddScoped(sp => new HttpClient { BaseAddress = new Uri(builder.HostEnvironment.BaseAddress) });

// Register Syncfusion Blazor Service
builder.Services.AddSyncfusionBlazor();

await builder.Build().RunAsync();
```

## Adding Theme Resources

### Include Stylesheet and Script

Open `~/index.html` and add the theme stylesheet and script references in the `<head>` section:

```html
<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Blazor Menu Bar</title>
    
    <!-- Syncfusion Blazor Bootstrap5 Theme -->
    <link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
    
    <!-- Syncfusion Blazor Core Script -->
    <script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js" type="text/javascript"></script>
    
    <link href="app.css" rel="stylesheet" />
</head>
```

### Available Theme Options

Syncfusion provides multiple themes. Choose one by changing the stylesheet:
- `bootstrap5.css` - Modern Bootstrap 5 theme
- `material.css` - Material Design theme
- `fabric.css` - Microsoft Fabric theme
- `tailwind.css` - Tailwind CSS theme
- `fluent.css` - Microsoft Fluent UI theme

For more information, see [Blazor Themes documentation](https://blazor.syncfusion.com/documentation/appearance/themes).

## Creating Your First Menu

### Basic Menu Structure

Add the following code to your `~/Pages/Index.razor` file:

```razor
@page "/"
@using Syncfusion.Blazor.Navigations

<SfMenu TValue="MenuItem">
    <MenuItems>
        <MenuItem Text="File">
            <MenuItems>
                <MenuItem Text="Open"></MenuItem>
                <MenuItem Text="Save"></MenuItem>
                <MenuItem Separator="true"></MenuItem>
                <MenuItem Text="Exit"></MenuItem>
            </MenuItems>
        </MenuItem>
        <MenuItem Text="Edit">
            <MenuItems>
                <MenuItem Text="Cut"></MenuItem>
                <MenuItem Text="Copy"></MenuItem>
                <MenuItem Text="Paste"></MenuItem>
            </MenuItems>
        </MenuItem>
        <MenuItem Text="View">
            <MenuItems>
                <MenuItem Text="Toolbars"></MenuItem>
                <MenuItem Text="Zoom"></MenuItem>
                <MenuItem Text="Full Screen"></MenuItem>
            </MenuItems>
        </MenuItem>
        <MenuItem Text="Help"></MenuItem>
    </MenuItems>
</SfMenu>
```

### Key Components

- **`SfMenu`**: Main container component with `TValue` specifying the data type
- **`MenuItems`**: Container for menu item collection
- **`MenuItem`**: Individual menu item with `Text` property
- **Nested `MenuItems`**: Submenus under parent MenuItem

### Understanding TValue

The `TValue` property specifies the data type for menu items. For simple menus, use the built-in `MenuItem` class. For complex scenarios with data binding, use your custom class.

## Verification

Run your application:

```bash
dotnet run
```

Navigate to `https://localhost:xxxx` in your browser. You should see a horizontal menu bar with File, Edit, View, and Help items. Hover over items with submenus to see the dropdown appear.

## Next Steps

1. **Add Icons**: Use `IconCss` property to display icons
2. **Handle Events**: Implement `MenuEvents` to respond to user interactions
3. **Bind Data**: Use `MenuFieldSettings` for dynamic menu creation
4. **Customize Appearance**: Apply custom CSS and animations
5. **Advanced Scenarios**: Explore multi-level nesting and templates

## Troubleshooting

**Menu not appearing?**
- Verify NuGet packages are installed: `dotnet restore`
- Check that Syncfusion service is registered in `Program.cs`
- Ensure theme stylesheet is linked in `index.html`
- Verify namespaces are imported in `_Imports.razor`

**Styling issues?**
- Confirm the correct theme CSS is referenced
- Check for conflicting CSS rules
- Inspect browser developer tools for theme file loading

**Build errors?**
- Ensure all NuGet packages have matching versions
- Clean and rebuild: `dotnet clean && dotnet build`
- Update to latest package versions if needed
