# Getting Started with Blazor Breadcrumb

## Table of Contents
- [NuGet Package Installation](#nuget-package-installation)
- [Namespace Imports](#namespace-imports)
- [Service Registration](#service-registration)
- [Theme Configuration](#theme-configuration)
- [Basic Component Initialization](#basic-component-initialization)
- [First Render](#first-render)

---

## NuGet Package Installation

Install the required NuGet packages for Syncfusion Blazor Breadcrumb component:

```
Install-Package Syncfusion.Blazor.Navigations -Version 28.x.x
Install-Package Syncfusion.Blazor.Themes -Version 28.x.x
```

Or use the .NET CLI:

```
dotnet add package Syncfusion.Blazor.Navigations
dotnet add package Syncfusion.Blazor.Themes
```

**Note:** Replace `28.x.x` with the latest available version. See [NuGet packages](https://www.nuget.org/packages?q=syncfusion.blazor) for version information.

---

## Namespace Imports

After package installation, open the **~/_Imports.razor** file and add the Syncfusion Blazor namespaces:

```csharp
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Navigations
```

This makes the `SfBreadcrumb`, `BreadcrumbItem`, and related components available throughout your Blazor application.

---

## Service Registration

Register the Syncfusion Blazor service in **Program.cs**:

```csharp
using Syncfusion.Blazor;

// ...

builder.Services.AddSyncfusionBlazor();
```

This initialization enables all Syncfusion components, including the Breadcrumb control.

---

## Theme Configuration

Add the theme stylesheet and JavaScript interop script to your **~/index.html** file (for WebAssembly) or **~/Pages/_Layout.cshtml** (for Server):

```html
<!-- In the <head> section: -->
<link href="_content/Syncfusion.Blazor.Themes/fluent2.css" rel="stylesheet" />

<!-- Before closing </body> tag: -->
<script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js" 
        type="text/javascript"></script>
```

**Available themes:** `fluent2`, `bootstrap5`, `bootstrap`, `material`, `fabric`, `tailwind`, etc.

For more theme options, refer to [Blazor Themes documentation](https://blazor.syncfusion.com/documentation/appearance/themes).

---

## Basic Component Initialization

Create a Blazor component with the `SfBreadcrumb` control:

```cshtml
@using Syncfusion.Blazor.Navigations

<SfBreadcrumb></SfBreadcrumb>
```

This creates a minimal breadcrumb that will auto-generate items based on the current URL.

---

## First Render

When the breadcrumb renders, it automatically parses the current page URL and creates breadcrumb items:

```cshtml
@using Syncfusion.Blazor.Navigations

<SfBreadcrumb></SfBreadcrumb>
```

**Example:** If your URL is `/products/electronics/laptops`, the breadcrumb will display:
- `products` > `electronics` > `laptops`

Each segment becomes a clickable breadcrumb item that navigates to its corresponding URL.

To add custom items instead of URL-based auto-generation, use the `BreadcrumbItems` container:

```cshtml
@using Syncfusion.Blazor.Navigations

<SfBreadcrumb>
    <BreadcrumbItems>
        <BreadcrumbItem IconCss="e-icons e-home" Url="/"></BreadcrumbItem>
        <BreadcrumbItem Text="Products" Url="/products"></BreadcrumbItem>
        <BreadcrumbItem Text="Current Product"></BreadcrumbItem>
    </BreadcrumbItems>
</SfBreadcrumb>
```

This manual approach gives you full control over breadcrumb content and URLs.
