# Getting Started with Blazor Toolbar Component

## Table of Contents
- [Overview](#overview)
- [Installation](#installation)
  - [Install NuGet Packages](#install-nuget-packages)
  - [Register Syncfusion Services](#register-syncfusion-services)
  - [Add Namespace Imports](#add-namespace-imports)
  - [Add Stylesheet and Script References](#add-stylesheet-and-script-references)
- [Create Your First Toolbar](#create-your-first-toolbar)
- [Basic Toolbar Structure](#basic-toolbar-structure)
- [Understanding ToolbarItems](#understanding-toolbaritems)
- [Adding Icons to Toolbar Items](#adding-icons-to-toolbar-items)
- [Adding Separators](#adding-separators)
- [Setting Toolbar Width](#setting-toolbar-width)
- [Complete Working Example](#complete-working-example)
- [Next Steps](#next-steps)

## Overview

The Syncfusion Blazor Toolbar component creates interactive command bars with buttons, icons, separators, and custom controls. This guide walks through installation, configuration, and creating your first toolbar.

## Installation

### Install NuGet Packages

Install the required NuGet packages for Toolbar and themes.

**Using Package Manager Console:**

```bash
Install-Package Syncfusion.Blazor.Navigations -Version {{ site.releaseversion }}
Install-Package Syncfusion.Blazor.Themes -Version {{ site.releaseversion }}
```

**Using .NET CLI:**

```bash
dotnet add package Syncfusion.Blazor.Navigations -v {{ site.releaseversion }}
dotnet add package Syncfusion.Blazor.Themes -v {{ site.releaseversion }}
dotnet restore
```

### Register Syncfusion Services

Open `~/Program.cs` and register the Syncfusion Blazor service:

```csharp
using Syncfusion.Blazor;

var builder = WebAssemblyHostBuilder.CreateDefault(args);
builder.RootComponents.Add<App>("#app");
builder.RootComponents.Add<HeadOutlet>("head::after");

builder.Services.AddScoped(sp => new HttpClient { BaseAddress = new Uri(builder.HostEnvironment.BaseAddress) });

// Register Syncfusion Blazor service
builder.Services.AddSyncfusionBlazor();

await builder.Build().RunAsync();
```

### Add Namespace Imports

Open `~/_Imports.razor` and add the required namespaces:

```razor
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Navigations
```

### Add Stylesheet and Script References

Add theme stylesheet and script references in `~/index.html` (WebAssembly) or `~/Pages/_Host.cshtml` (Server):

```html
<head>
    <!-- Syncfusion Blazor theme -->
    <link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
</head>

<body>
    <!-- Syncfusion Blazor script -->
    <script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js" type="text/javascript"></script>
</body>
```

**Available Themes:**
- `bootstrap5.css` - Bootstrap 5 theme (recommended)
- `material.css` - Material theme
- `fabric.css` - Fluent theme
- `tailwind.css` - Tailwind CSS theme
- `fluent.css` - Microsoft Fluent UI theme

## Create Your First Toolbar

Add a basic toolbar in any Razor page (e.g., `~/Pages/Index.razor`):

```razor
<SfToolbar>
    <ToolbarItems>
        <ToolbarItem Text="Cut"></ToolbarItem>
        <ToolbarItem Text="Copy"></ToolbarItem>
        <ToolbarItem Text="Paste"></ToolbarItem>
        <ToolbarItem Text="Bold"></ToolbarItem>
        <ToolbarItem Text="Underline"></ToolbarItem>
        <ToolbarItem Text="Italic"></ToolbarItem>
    </ToolbarItems>
</SfToolbar>
```

This creates a simple toolbar with six text buttons.

## Basic Toolbar Structure

The Toolbar component follows this structure:

```razor
<SfToolbar>
    <ToolbarItems>
        <!-- Individual toolbar items go here -->
        <ToolbarItem Text="Button 1"></ToolbarItem>
        <ToolbarItem Text="Button 2"></ToolbarItem>
    </ToolbarItems>
</SfToolbar>
```

**Key Components:**
- `<SfToolbar>` - The main toolbar container
- `<ToolbarItems>` - Collection wrapper for toolbar items
- `<ToolbarItem>` - Individual buttons or controls

## Understanding ToolbarItems

Each `ToolbarItem` represents a button or control in the toolbar. The most basic property is `Text`:

```razor
<ToolbarItem Text="Cut"></ToolbarItem>
```

You can configure items with multiple properties:

```razor
<ToolbarItem 
    Text="Cut" 
    TooltipText="Cut selected text"
    Disabled="false"
    Visible="true">
</ToolbarItem>
```

## Adding Icons to Toolbar Items

Use the `PrefixIcon` property to add icons before text:

```razor
<SfToolbar>
    <ToolbarItems>
        <ToolbarItem Text="Cut" PrefixIcon="e-icons e-cut"></ToolbarItem>
        <ToolbarItem Text="Copy" PrefixIcon="e-icons e-copy"></ToolbarItem>
        <ToolbarItem Text="Paste" PrefixIcon="e-icons e-paste"></ToolbarItem>
        <ToolbarItem Text="Bold" PrefixIcon="e-icons e-bold"></ToolbarItem>
        <ToolbarItem Text="Italic" PrefixIcon="e-icons e-italic"></ToolbarItem>
    </ToolbarItems>
</SfToolbar>
```

**Icon-Only Buttons:**

Omit the `Text` property to show only the icon:

```razor
<ToolbarItem PrefixIcon="e-icons e-cut" TooltipText="Cut"></ToolbarItem>
<ToolbarItem PrefixIcon="e-icons e-copy" TooltipText="Copy"></ToolbarItem>
<ToolbarItem PrefixIcon="e-icons e-paste" TooltipText="Paste"></ToolbarItem>
```

**Important:** Always provide `TooltipText` for icon-only buttons to ensure accessibility.

**Suffix Icons:**

Use `SuffixIcon` to place icons after text:

```razor
<ToolbarItem Text="Upload" SuffixIcon="e-icons e-upload-1"></ToolbarItem>
<ToolbarItem Text="Download" SuffixIcon="e-icons e-download"></ToolbarItem>
```

## Adding Separators

Use separators to visually group related items:

```razor
<SfToolbar>
    <ToolbarItems>
        <ToolbarItem Text="Cut" PrefixIcon="e-icons e-cut"></ToolbarItem>
        <ToolbarItem Text="Copy" PrefixIcon="e-icons e-copy"></ToolbarItem>
        <ToolbarItem Text="Paste" PrefixIcon="e-icons e-paste"></ToolbarItem>
        
        <!-- Separator between groups -->
        <ToolbarItem Type="ItemType.Separator"></ToolbarItem>
        
        <ToolbarItem Text="Bold" PrefixIcon="e-icons e-bold"></ToolbarItem>
        <ToolbarItem Text="Italic" PrefixIcon="e-icons e-italic"></ToolbarItem>
        <ToolbarItem Text="Underline" PrefixIcon="e-icons e-underline"></ToolbarItem>
    </ToolbarItems>
</SfToolbar>
```

**Note:** Separators at the beginning or end of the toolbar are not visible.

## Setting Toolbar Width

Control toolbar width using the `Width` property:

```razor
<!-- Fixed width -->
<SfToolbar Width="600px">
    <ToolbarItems>
        <ToolbarItem Text="Cut"></ToolbarItem>
        <ToolbarItem Text="Copy"></ToolbarItem>
    </ToolbarItems>
</SfToolbar>

<!-- Percentage width -->
<SfToolbar Width="100%">
    <ToolbarItems>
        <ToolbarItem Text="Cut"></ToolbarItem>
        <ToolbarItem Text="Copy"></ToolbarItem>
    </ToolbarItems>
</SfToolbar>

<!-- Auto width (default) -->
<SfToolbar>
    <ToolbarItems>
        <ToolbarItem Text="Cut"></ToolbarItem>
        <ToolbarItem Text="Copy"></ToolbarItem>
    </ToolbarItems>
</SfToolbar>
```

## Complete Working Example

Here's a complete example with common toolbar patterns:

```razor
@page "/toolbar-demo"
@using Syncfusion.Blazor.Navigations

<h3>Text Editor Toolbar</h3>

<SfToolbar Width="800px">
    <ToolbarItems>
        <!-- Clipboard group -->
        <ToolbarItem Text="Cut" PrefixIcon="e-icons e-cut" TooltipText="Cut"></ToolbarItem>
        <ToolbarItem Text="Copy" PrefixIcon="e-icons e-copy" TooltipText="Copy"></ToolbarItem>
        <ToolbarItem Text="Paste" PrefixIcon="e-icons e-paste" TooltipText="Paste"></ToolbarItem>
        <ToolbarItem Type="ItemType.Separator"></ToolbarItem>
        
        <!-- Formatting group -->
        <ToolbarItem Text="Bold" PrefixIcon="e-icons e-bold" TooltipText="Bold"></ToolbarItem>
        <ToolbarItem Text="Italic" PrefixIcon="e-icons e-italic" TooltipText="Italic"></ToolbarItem>
        <ToolbarItem Text="Underline" PrefixIcon="e-icons e-underline" TooltipText="Underline"></ToolbarItem>
        <ToolbarItem Type="ItemType.Separator"></ToolbarItem>
        
        <!-- Alignment group -->
        <ToolbarItem Text="Left" PrefixIcon="e-icons e-align-left" TooltipText="Align Left"></ToolbarItem>
        <ToolbarItem Text="Center" PrefixIcon="e-icons e-align-center" TooltipText="Align Center"></ToolbarItem>
        <ToolbarItem Text="Right" PrefixIcon="e-icons e-align-right" TooltipText="Align Right"></ToolbarItem>
        <ToolbarItem Text="Justify" PrefixIcon="e-icons e-justify" TooltipText="Justify"></ToolbarItem>
    </ToolbarItems>
</SfToolbar>
```

## Next Steps

Now that you have a basic toolbar running, explore these topics:

1. **Item Configuration** - Learn about all 18 toolbar item properties (Align, Disabled, Visible, Width, etc.)
2. **How-To Guides** - Add, remove, enable, disable items dynamically
3. **Responsive Modes** - Handle overflow with Scrollable, Popup, MultiRow, or Extended modes
4. **Alignment and Spacing** - Use Spacer for flexible item positioning
5. **Accessibility** - Implement keyboard navigation and ARIA support
6. **Styling** - Customize toolbar appearance with CSS

Refer to the main SKILL.md navigation guide to access detailed documentation for each topic.
