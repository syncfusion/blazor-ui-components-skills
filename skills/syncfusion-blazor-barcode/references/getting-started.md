# Getting Started with Syncfusion Blazor Barcode Generator

## Table of Contents
- [Prerequisites](#prerequisites)
- [Install NuGet Packages](#install-nuget-packages)
- [Configure Namespaces](#configure-namespaces)
- [Register Syncfusion Service](#register-syncfusion-service)
- [Add Stylesheet and Script Resources](#add-stylesheet-and-script-resources)
- [Add Barcode Component to a Page](#add-barcode-component-to-a-page)
- [Project-Type Differences](#project-type-differences)
- [Render Modes (Blazor Web App .NET 8+)](#render-modes-blazor-web-app-net-8)
- [Troubleshooting Setup](#troubleshooting-setup)

---

## Prerequisites

- .NET SDK (latest recommended): https://dotnet.microsoft.com/en-us/download
- [System requirements for Syncfusion Blazor components](https://blazor.syncfusion.com/documentation/system-requirements)

---

## Install NuGet Packages

Install both packages — `BarcodeGenerator` for the components and `Themes` for CSS:

**Visual Studio (Package Manager Console):**
```bash
Install-Package Syncfusion.Blazor.BarcodeGenerator
Install-Package Syncfusion.Blazor.Themes
```

**dotnet CLI:**
```bash
dotnet add package Syncfusion.Blazor.BarcodeGenerator
dotnet add package Syncfusion.Blazor.Themes
dotnet restore
```

> **Blazor Web App (WebAssembly / Auto render modes):** Install packages in the **client** `.Client` project, not the server project.

---

## Configure Namespaces

Open `~/_Imports.razor` and add:

```csharp
@using Syncfusion.Blazor
@using Syncfusion.Blazor.BarcodeGenerator
```

> For Blazor Web App with WebAssembly/Auto render modes, add this to `_Imports.razor` in the **client** project.

---

## Register Syncfusion Service

### Blazor WebAssembly — `~/Program.cs`

```csharp
using Microsoft.AspNetCore.Components.WebAssembly.Hosting;
using Syncfusion.Blazor;

var builder = WebAssemblyHostBuilder.CreateDefault(args);
builder.Services.AddSyncfusionBlazor();
await builder.Build().RunAsync();
```

### Blazor Server App — `~/Program.cs`

```csharp
using Syncfusion.Blazor;

var builder = WebApplication.CreateBuilder(args);
builder.Services.AddRazorPages();
builder.Services.AddServerSideBlazor();
builder.Services.AddSyncfusionBlazor();

var app = builder.Build();
// ...
```

### Blazor Web App (.NET 8+) — Both Projects

**Server `~/Program.cs`:**
```csharp
using Syncfusion.Blazor;

var builder = WebApplication.CreateBuilder(args);
builder.Services.AddRazorComponents()
    .AddInteractiveServerComponents()
    .AddInteractiveWebAssemblyComponents();
builder.Services.AddSyncfusionBlazor();
```

**Client `~/Program.cs`:**
```csharp
using Syncfusion.Blazor;

var builder = WebAssemblyHostBuilder.CreateDefault(args);
builder.Services.AddSyncfusionBlazor();
await builder.Build().RunAsync();
```

---

## Add Stylesheet and Script Resources

### Blazor WebAssembly — `~/wwwroot/index.html`

```html
<head>
    <link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
    <script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js"
            type="text/javascript"></script>
</head>
```

### Blazor Server App / Blazor Web App — `~/Components/App.razor`

```html
<head>
    <link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
</head>
<body>
    <!-- ... -->
    <script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js"
            type="text/javascript"></script>
</body>
```

**Available themes:** `bootstrap5.css`, `material.css`, `fluent.css`, `tailwind.css`, `fabric.css`

---

## Add Barcode Component to a Page

Add any of the three generators to a `.razor` page (e.g., `~/Pages/Home.razor`):

```razor
@using Syncfusion.Blazor.BarcodeGenerator

<!-- 1D Barcode -->
<SfBarcodeGenerator Width="@width" Height="@height"
                    Type="@BarcodeType.Code128"
                    Value="SYNCFUSION">
</SfBarcodeGenerator>
@code{
    string width = "200px";
    string height = "200px";
}

<!-- QR Code -->
<SfQRCodeGenerator Width="@qrWidth" Height="@qrHeight"
                   Value="https://www.syncfusion.com">
</SfQRCodeGenerator>
@code{
    string qrWidth = "200px";
    string qrHeight = "200px";
}

<!-- Data Matrix -->
<SfDataMatrixGenerator Width="@width" Height="@height"
                       Value="SYNCFUSION">
</SfDataMatrixGenerator>
@code{
    string width = "200px";
    string height = "200px";
}

```

---

## Project-Type Differences

| Setup Step | Blazor WASM | Blazor Server | Blazor Web App |
|-----------|-------------|---------------|----------------|
| NuGet install location | Client project | Server project | Client project (for WASM/Auto) |
| `AddSyncfusionBlazor()` | Client `Program.cs` | Server `Program.cs` | Both `Program.cs` files |
| Script in `<head>` or `<body>` | `<head>` (index.html) | End of `<body>` (App.razor) | End of `<body>` (App.razor) |
| `_Imports.razor` location | Client project root | Project root | Client project root |

---

## Render Modes (Blazor Web App .NET 8+)

When **Interactivity Location** is set to **Per page/component**, declare the render mode at the top of your `.razor` file:

```razor
@* Choose one render mode: *@
@rendermode InteractiveServer
@rendermode InteractiveWebAssembly
@rendermode InteractiveAuto
```

When **Interactivity Location** is **Global**, the render mode is set in `App.razor` and does not need to be declared per page.

---

## Troubleshooting Setup

**Component not rendering / blank area:**
- Confirm `AddSyncfusionBlazor()` is called in `Program.cs`
- Verify `@using Syncfusion.Blazor.BarcodeGenerator` is in `_Imports.razor`
- Check render mode is configured for .NET 8+ Web App

**Styles not applied:**
- Confirm the theme CSS `<link>` is in `<head>`
- Check for conflicting global CSS overrides

**Script errors in console:**
- Confirm `syncfusion-blazor.min.js` is referenced and loading correctly
- For Server apps, place script at end of `<body>` in `App.razor`

**License warning banner:**
```csharp
// Add in Program.cs before builder.Build()
Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense("YOUR-LICENSE-KEY");
```
Get a free community license at: https://www.syncfusion.com/sales/products
