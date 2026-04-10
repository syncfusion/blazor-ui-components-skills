# Getting Started with Blazor AppBar Component

## Table of Contents
- [Overview](#overview)
- [Install Syncfusion Blazor Packages](#install-syncfusion-blazor-packages)
- [Add Import Namespaces](#add-import-namespaces)
- [Register Syncfusion Blazor Service](#register-syncfusion-blazor-service)
- [Add Stylesheet and Script Resources](#add-stylesheet-and-script-resources)
- [Add Blazor AppBar Component](#add-blazor-appbar-component)

## Overview

This guide walks through adding the Syncfusion Blazor AppBar component to a Blazor WebAssembly application. The AppBar component provides a navigation bar for displaying branding, navigation, and actions.

## Install Syncfusion Blazor Packages

Install the required NuGet packages for the AppBar component:

**Using NuGet Package Manager (Visual Studio):**
1. Open **Tools → NuGet Package Manager → Manage NuGet Packages for Solution**
2. Search for and install:
   - `Syncfusion.Blazor.Navigations`
   - `Syncfusion.Blazor.Themes`

**Using Package Manager Console:**

```bash
Install-Package Syncfusion.Blazor.Navigations -Version {{ site.releaseversion }}
Install-Package Syncfusion.Blazor.Themes -Version {{ site.releaseversion }}
```

**Using .NET CLI or VS Code Terminal:**

```bash
dotnet add package Syncfusion.Blazor.Navigations
dotnet add package Syncfusion.Blazor.Themes
```

> **Note:** All Syncfusion Blazor packages are available on [nuget.org](https://www.nuget.org/packages?q=syncfusion.blazor). See the NuGet packages topic for details.

## Add Import Namespaces

After installing the packages, open the **~/_Imports.razor** file and import the required namespaces:

```csharp
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Navigations
```

These namespaces provide access to the AppBar component and related types.

## Register Syncfusion Blazor Service

Register the Syncfusion Blazor Service in the **Program.cs** file of your Blazor WebAssembly App:

```csharp
using Syncfusion.Blazor;

var builder = WebAssemblyHostBuilder.CreateDefault(args);
// ... other configurations

builder.Services.AddSyncfusionBlazor();

await builder.Build().RunAsync();
```

This registration enables all Syncfusion Blazor components in your application.

## Add Stylesheet and Script Resources

The theme stylesheet and script can be accessed from NuGet through Static Web Assets. Include the stylesheet and script references in the **~/index.html** file:

```html
<head>
    <!-- ... other head content -->
    <link href="_content/Syncfusion.Blazor.Themes/fluent2.css" rel="stylesheet" />
</head>
<body>
    <!-- ... body content -->
    <script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js" type="text/javascript"></script>
</body>
```

**Available Themes:**
- `fluent2.css` - Fluent 2 theme (shown above)
- `bootstrap5.css` - Bootstrap 5 theme
- `material.css` - Material theme
- `fabric.css` - Fabric (Office 365) theme
- `tailwind.css` - Tailwind CSS theme
- `highcontrast.css` - High contrast theme

> **Note:** Check out the Blazor Themes documentation to discover various methods (Static Web Assets, CDN, and CRG) for referencing themes. Also see the Adding Script Reference topic for different approaches to adding script references.

## Add Blazor AppBar Component

Add the Syncfusion Blazor AppBar component to a Razor page (e.g., **~/Pages/Index.razor**):

```cshtml
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.Buttons

<SfAppBar ColorMode="AppBarColor.Primary">
    <SfButton CssClass="e-inherit" IconCss="e-icons e-menu"></SfButton>
    <span class="regular">Blazor AppBar</span>
    <AppBarSpacer></AppBarSpacer>
    <SfButton CssClass="e-inherit" Content="FREE TRIAL"></SfButton>
</SfAppBar>

<style>
    .e-btn.e-inherit {
        margin: 0 3px;
    }
</style>
```

**Run the Application:**
- Press **Ctrl+F5** (Windows) or **⌘+F5** (macOS) to launch the application
- The Syncfusion Blazor AppBar component will render in the default web browser

### Understanding the Basic Structure

**SfAppBar Component:**
- Container for the AppBar with configuration properties
- `ColorMode` property sets the background color scheme

**SfButton with e-inherit:**
- Buttons inherit the AppBar's color scheme using `CssClass="e-inherit"`
- `IconCss` applies Syncfusion icon fonts
- `Content` property sets button text

**AppBarSpacer:**
- Creates flexible spacing between elements
- Pushes subsequent elements to the right
- Allows for responsive layouts

### Adding More Elements

You can add various child elements to the AppBar:

```cshtml
<SfAppBar ColorMode="AppBarColor.Primary">
    <!-- Menu button -->
    <SfButton CssClass="e-inherit" IconCss="e-icons e-menu"></SfButton>
    
    <!-- Title/Brand -->
    <span class="app-title">My Application</span>
    
    <!-- Navigation buttons -->
    <SfButton CssClass="e-inherit" Content="Home"></SfButton>
    <SfButton CssClass="e-inherit" Content="About"></SfButton>
    
    <!-- Spacer pushes remaining items to the right -->
    <AppBarSpacer></AppBarSpacer>
    
    <!-- Action buttons on the right -->
    <SfButton CssClass="e-inherit" IconCss="e-icons e-settings"></SfButton>
    <SfButton CssClass="e-inherit" Content="Login"></SfButton>
</SfAppBar>
```

### Next Steps

Now that you have a basic AppBar running:

- Explore different **size modes** (Regular, Prominent, Dense) in the size-and-color.md reference
- Configure **color modes** (Light, Dark, Primary, Inherit) in the size-and-color.md reference
- Set up **positioning** (Top, Bottom, Sticky) in the positioning.md reference
- Add **advanced UI elements** (Spacer, Separator, Menu, Sidebar) in the design-ui.md reference
- Implement **custom styling** with the styling.md reference
