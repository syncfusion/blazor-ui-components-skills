# Getting Started with Blazor Chart Wizard in WebAssembly App

This guide provides complete step-by-step instructions for setting up and using the Syncfusion Blazor Chart Wizard component in a Blazor WebAssembly (WASM) application.

## Table of Contents

- [Overview](#overview)
- [Prerequisites](#prerequisites)
- [Creating Blazor WASM App](#creating-blazor-wasm-app)
  - [Using Visual Studio](#using-visual-studio)
  - [Using Visual Studio Code](#using-visual-studio-code)
  - [Using .NET CLI](#using-net-cli)
- [Installing NuGet Packages](#installing-nuget-packages)
- [Configuration Steps](#configuration-steps)
  - [Adding Namespace Imports](#adding-namespace-imports)
  - [Registering Syncfusion Blazor Service](#registering-syncfusion-blazor-service)
  - [Adding Script References](#adding-script-references)
- [Adding Chart Wizard Component](#adding-chart-wizard-component)
- [Data Binding](#data-binding)
- [Themes in WASM](#themes-in-wasm)
- [Complete WASM Example](#complete-wasm-example)
- [Running the Application](#running-the-application)
- [WASM-Specific Considerations](#wasm-specific-considerations)
- [Troubleshooting](#troubleshooting)

## Overview

Blazor WebAssembly runs entirely in the browser using WebAssembly, making it ideal for offline-capable applications. This guide covers setting up the Chart Wizard component specifically for WASM scenarios with performance considerations.

## Prerequisites

Before starting, ensure you have:

- **.NET SDK 6.0 or later** - [Download here](https://dotnet.microsoft.com/download)
- **Visual Studio 2022** (or VS Code with C# extension)
- **Syncfusion Blazor license** - [Get trial license](https://www.syncfusion.com/downloads/blazor)
- Basic knowledge of Blazor WebAssembly architecture

Verify your .NET SDK installation:

```bash
dotnet --version
```

## Creating Blazor WASM App

Choose your preferred development environment to create a Blazor WebAssembly application.

### Using Visual Studio

1. Open Visual Studio 2022
2. Click **Create a new project**
3. Search for **Blazor WebAssembly App** template
4. Select it and click **Next**
5. Enter Project name: `BlazorChartWizardWasm`
6. Choose location and click **Next**
7. Configure project settings:
   - **Framework**: .NET 8.0 (or later)
   - **Authentication type**: None
   - **Configure for HTTPS**: Checked
   - **ASP.NET Core hosted**: Optional (unchecked for standalone WASM)
   - **Progressive Web Application**: Optional
8. Click **Create**

![Blazor WASM Create Project Template](../images/blazor-wasm-app-template.png)

### Using Visual Studio Code

1. Open VS Code
2. Press <kbd>Ctrl</kbd>+<kbd>`</kbd> to open integrated terminal
3. Run the following commands:

```bash
dotnet new blazorwasm -o BlazorChartWizardWasm
cd BlazorChartWizardWasm
code .
```

This creates a standalone Blazor WASM app and opens it in VS Code.

### Using .NET CLI

Open a terminal and run:

```bash
dotnet new blazorwasm -o BlazorChartWizardWasm
cd BlazorChartWizardWasm
```

## Installing NuGet Packages

Install the Syncfusion Blazor Chart Wizard package using your preferred method.

### Visual Studio

1. Go to **Tools** → **NuGet Package Manager** → **Manage NuGet Packages for Solution**
2. Click **Browse** tab
3. Search for `Syncfusion.Blazor.ChartWizard`
4. Select the package
5. Check your project
6. Click **Install**
7. Accept the license terms

### Package Manager Console

In Visual Studio, open **Tools** → **NuGet Package Manager** → **Package Manager Console** and run:

```powershell
Install-Package Syncfusion.Blazor.ChartWizard -Version 27.1.48
```

### .NET CLI / VS Code

In your terminal, run:

```bash
dotnet add package Syncfusion.Blazor.ChartWizard
dotnet restore
```

**Important:** In WASM apps, the package is added to the client project. If you're using an ASP.NET Core hosted model, ensure you add the package to the `.Client` project, not the `.Server` project.

## Configuration Steps

After installing the package, configure your Blazor WASM app to use the Chart Wizard component.

### Adding Namespace Imports

Open the `_Imports.razor` file (located in the project root) and add these namespaces:

```cshtml
@using System.Net.Http
@using System.Net.Http.Json
@using Microsoft.AspNetCore.Components.Forms
@using Microsoft.AspNetCore.Components.Routing
@using Microsoft.AspNetCore.Components.Web
@using Microsoft.AspNetCore.Components.Web.Virtualization
@using Microsoft.AspNetCore.Components.WebAssembly.Http
@using Microsoft.JSInterop
@using BlazorChartWizardWasm
@using BlazorChartWizardWasm.Layout

@* Add Syncfusion namespaces *@
@using Syncfusion.Blazor
@using Syncfusion.Blazor.ChartWizard
```

### Registering Syncfusion Blazor Service

Open `Program.cs` and register the Syncfusion Blazor service:

```csharp
using Microsoft.AspNetCore.Components.Web;
using Microsoft.AspNetCore.Components.WebAssembly.Hosting;
using BlazorChartWizardWasm;
using Syncfusion.Blazor;

var builder = WebAssemblyHostBuilder.CreateDefault(args);
builder.RootComponents.Add<App>("#app");
builder.RootComponents.Add<HeadOutlet>("head::after");

builder.Services.AddScoped(sp => new HttpClient 
{ 
    BaseAddress = new Uri(builder.HostEnvironment.BaseAddress) 
});

// Register Syncfusion Blazor service
builder.Services.AddSyncfusionBlazor();

await builder.Build().RunAsync();
```

**Key lines:**
```csharp
using Syncfusion.Blazor;
builder.Services.AddSyncfusionBlazor();
```

**Optional - Register License:**

If you have a Syncfusion license key, register it before `builder.Build()`:

```csharp
// Register Syncfusion license
Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense("YOUR_LICENSE_KEY");

builder.Services.AddSyncfusionBlazor();

await builder.Build().RunAsync();
```

### Adding Script References

Open `wwwroot/index.html` and add the Syncfusion script reference at the end of the `<body>` tag:

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>BlazorChartWizardWasm</title>
    <base href="/" />
    <link rel="stylesheet" href="css/bootstrap/bootstrap.min.css" />
    <link rel="stylesheet" href="css/app.css" />
    <link rel="icon" type="image/png" href="favicon.png" />
    <link href="BlazorChartWizardWasm.styles.css" rel="stylesheet" />
</head>

<body>
    <div id="app">
        <svg class="loading-progress">
            <circle r="40%" cx="50%" cy="50%" />
            <circle r="40%" cx="50%" cy="50%" />
        </svg>
        <div class="loading-progress-text"></div>
    </div>

    <div id="blazor-error-ui">
        An unhandled error has occurred.
        <a href="" class="reload">Reload</a>
        <a class="dismiss">🗙</a>
    </div>
    
    <script src="_framework/blazor.webassembly.js"></script>
    
    <!-- Add Syncfusion Blazor script reference -->
    <script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js" type="text/javascript"></script>
</body>

</html>
```

**Important:** The Syncfusion script must come after `blazor.webassembly.js`.

## Adding Chart Wizard Component

Now you can add the Chart Wizard component to your Blazor pages.

### Basic Component

Open `Pages/Home.razor` (or `Pages/Index.razor` in some templates) and add:

```cshtml
@page "/"

<PageTitle>Chart Wizard</PageTitle>

<h1>Blazor WASM Chart Wizard</h1>
<p>Interactive chart creation running entirely in the browser</p>

<SfChartWizard>
    
</SfChartWizard>
```

**Note:** Unlike Blazor Server, WASM apps don't require `@rendermode` attribute since all components run on the client by default.

## Data Binding

### Creating Data Model

Add your data model in the `@code` block:

```csharp
@code {
    public class OlympicsData
    {
        public string? Country { get; set; }
        public string? CountryCode { get; set; }
        public int Gold { get; set; }
        public int Silver { get; set; }
        public int Bronze { get; set; }
    }
}
```

### Creating Data Collection

Add a data source:

```csharp
@code {
    private readonly List<string> chartSeries = new() { "Gold", "Silver", "Bronze" };
    private readonly List<string> categories = new() { "Country" };

    private readonly List<OlympicsData> OlympicsDataSource = new()
    {
        new OlympicsData { Country = "USA", CountryCode = "USA", Gold = 40, Silver = 44, Bronze = 42 },
        new OlympicsData { Country = "China", CountryCode = "CHN", Gold = 40, Silver = 27, Bronze = 24 },
        new OlympicsData { Country = "Great Britain", CountryCode = "GBR", Gold = 14, Silver = 22, Bronze = 29 },
        new OlympicsData { Country = "France", CountryCode = "FRA", Gold = 16, Silver = 26, Bronze = 22 },
        new OlympicsData { Country = "Australia", CountryCode = "AUS", Gold = 18, Silver = 19, Bronze = 16 },
        new OlympicsData { Country = "Japan", CountryCode = "JPN", Gold = 20, Silver = 12, Bronze = 13 },
        new OlympicsData { Country = "Italy", CountryCode = "ITA", Gold = 12, Silver = 13, Bronze = 15 },
        new OlympicsData { Country = "Netherlands", CountryCode = "NLD", Gold = 15, Silver = 7, Bronze = 12 },
        new OlympicsData { Country = "Germany", CountryCode = "DEU", Gold = 12, Silver = 13, Bronze = 8 },
        new OlympicsData { Country = "South Korea", CountryCode = "KOR", Gold = 13, Silver = 9, Bronze = 10 }
    };

    public class OlympicsData
    {
        public string? Country { get; set; }
        public string? CountryCode { get; set; }
        public int Gold { get; set; }
        public int Silver { get; set; }
        public int Bronze { get; set; }
    }
}
```

### Binding to Chart Wizard

Configure the Chart Wizard with your data:

```cshtml
<SfChartWizard>
    <ChartSettings DataSource="@OlympicsDataSource"
                   CategoryFields="@categories"
                   SeriesFields="@chartSeries"
                   SeriesType="ChartWizardSeriesType.Bar">
    </ChartSettings>
</SfChartWizard>
```

## Themes in WASM

### Adding Theme Stylesheet

Add the theme stylesheet reference in the `<head>` section of `wwwroot/index.html`:

```html
<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>BlazorChartWizardWasm</title>
    <base href="/" />
    <link rel="stylesheet" href="css/bootstrap/bootstrap.min.css" />
    <link rel="stylesheet" href="css/app.css" />
    
    <!-- Add Syncfusion theme stylesheet -->
    <link href="_content/Syncfusion.Blazor.Themes/fluent2.css" rel="stylesheet" />
    
    <link rel="icon" type="image/png" href="favicon.png" />
    <link href="BlazorChartWizardWasm.styles.css" rel="stylesheet" />
</head>
```

### Applying Theme

Set the `Theme` property on the component:

```cshtml
<SfChartWizard Theme="Theme.Fluent2">
    <ChartSettings DataSource="@OlympicsDataSource"
                   CategoryFields="@categories"
                   SeriesFields="@chartSeries"
                   SeriesType="ChartWizardSeriesType.Column">
    </ChartSettings>
</SfChartWizard>
```

**Available themes:**
- Material: `material3.css`, `material3-dark.css`
- Fluent: `fluent2.css`, `fluent2-dark.css`, `fluent2-highcontrast.css`
- Bootstrap: `bootstrap5.css`, `bootstrap5-dark.css`
- Tailwind: `tailwind3.css`, `tailwind3-dark.css`

## Complete WASM Example

Here's the complete `Home.razor` file for a Blazor WASM application:

```cshtml
@page "/"

<PageTitle>Chart Wizard WASM</PageTitle>

<div class="container mt-4">
    <h1>Olympic Medals Chart (WASM)</h1>
    <p>Running entirely in your browser with WebAssembly</p>
    
    <SfChartWizard Theme="Theme.Fluent2" Width="100%" Height="600px">
        <ChartSettings DataSource="@OlympicsDataSource"
                       CategoryFields="@categories"
                       SeriesFields="@chartSeries"
                       SeriesType="ChartWizardSeriesType.Column">
        </ChartSettings>
    </SfChartWizard>
</div>

@code {
    private readonly List<string> chartSeries = new() { "Gold", "Silver", "Bronze" };
    private readonly List<string> categories = new() { "Country" };

    private readonly List<OlympicsData> OlympicsDataSource = new()
    {
        new OlympicsData { Country = "USA", CountryCode = "USA", Gold = 40, Silver = 44, Bronze = 42 },
        new OlympicsData { Country = "China", CountryCode = "CHN", Gold = 40, Silver = 27, Bronze = 24 },
        new OlympicsData { Country = "Great Britain", CountryCode = "GBR", Gold = 14, Silver = 22, Bronze = 29 },
        new OlympicsData { Country = "France", CountryCode = "FRA", Gold = 16, Silver = 26, Bronze = 22 },
        new OlympicsData { Country = "Australia", CountryCode = "AUS", Gold = 18, Silver = 19, Bronze = 16 },
        new OlympicsData { Country = "Japan", CountryCode = "JPN", Gold = 20, Silver = 12, Bronze = 13 },
        new OlympicsData { Country = "Italy", CountryCode = "ITA", Gold = 12, Silver = 13, Bronze = 15 },
        new OlympicsData { Country = "Netherlands", CountryCode = "NLD", Gold = 15, Silver = 7, Bronze = 12 },
        new OlympicsData { Country = "Germany", CountryCode = "DEU", Gold = 12, Silver = 13, Bronze = 8 },
        new OlympicsData { Country = "South Korea", CountryCode = "KOR", Gold = 13, Silver = 9, Bronze = 10 }
    };

    public class OlympicsData
    {
        public string? Country { get; set; }
        public string? CountryCode { get; set; }
        public int Gold { get; set; }
        public int Silver { get; set; }
        public int Bronze { get; set; }
    }
}
```

## Running the Application

### Visual Studio

Press <kbd>F5</kbd> or <kbd>Ctrl</kbd>+<kbd>F5</kbd> to run the application.

**Note:** First run may take longer as WASM resources are downloaded and cached.

### VS Code / .NET CLI

In the terminal, run:

```bash
dotnet run
```

Or for better development experience with hot reload:

```bash
dotnet watch
```

Then open your browser to the URL shown (typically `https://localhost:5001`).

**First Launch:** The initial load will download the WASM runtime and assemblies. Subsequent visits will be much faster due to browser caching.

## WASM-Specific Considerations

### Performance Optimization

**Bundle Size:**
- WASM apps include the entire .NET runtime and application assemblies
- First load time is longer than Blazor Server
- Use browser caching effectively
- Consider lazy loading for large applications

**Initial Load Time:**
```
First visit: 3-5 seconds (downloading WASM + .NET runtime)
Cached visit: <1 second (instant from cache)
```

**Optimization Tips:**

1. **Enable Compression:**

Add to your web server configuration (IIS, nginx, Apache):

```xml
<!-- web.config for IIS -->
<staticContent>
    <mimeMap fileExtension=".blat" mimeType="application/octet-stream" />
    <mimeMap fileExtension=".dat" mimeType="application/octet-stream" />
</staticContent>
```

2. **Use AOT Compilation (optional):**

In your `.csproj` file:

```xml
<PropertyGroup>
    <RunAOTCompilation>true</RunAOTCompilation>
</PropertyGroup>
```

This increases build time but improves runtime performance.

3. **Lazy Loading:**

For larger applications, consider lazy loading routes:

```csharp
@using Microsoft.AspNetCore.Components.WebAssembly.Services

@inject LazyAssemblyLoader AssemblyLoader
```

### Offline Capability

WASM apps can work offline once cached:

1. Enable Progressive Web App (PWA) support:

```bash
dotnet new blazorwasm -o MyApp --pwa
```

2. Configure service worker for offline caching

3. Test offline functionality in browser DevTools (Network tab → Offline)

### Debugging

**Browser DevTools:**
- Press <kbd>F12</kbd> to open DevTools
- Use Sources tab to debug C# code
- Set breakpoints in .cs files
- Inspect variables and call stack

**Visual Studio Debugging:**
- Press <kbd>F5</kbd> to start debugging
- Set breakpoints in C# files
- Use standard debugging features

### Memory Considerations

WASM runs in the browser's memory space:

- **Typical memory usage:** 50-100MB for small apps
- **Monitor memory:** Use browser DevTools Performance tab
- **Dispose resources:** Implement `IDisposable` for components with heavy resources
- **Avoid memory leaks:** Unsubscribe from events in `Dispose()`

## Troubleshooting

### Long Initial Load Time

**Problem:** Application takes a long time to load on first visit.

**Solutions:**
1. **Enable compression:** Configure gzip/brotli compression on your web server
2. **Check network speed:** WASM downloads can be large (5-15MB)
3. **Use browser caching:** Ensure proper cache headers are set
4. **Consider Blazor Server:** For slow connections, Server mode may be better

### Chart Not Rendering

**Problem:** Component loads but chart doesn't display.

**Solutions:**
1. **Check browser console:** Press F12 and look for JavaScript errors
2. **Verify script reference:** Ensure `syncfusion-blazor.min.js` is loaded in `index.html`
3. **Check data binding:** Verify DataSource has valid data
4. **Test with sample data:** Use hardcoded data to isolate data binding issues

### 404 Errors for _content Files

**Problem:** Console shows 404 errors for `_content/Syncfusion.Blazor.xxx` files.

**Solutions:**
1. **Rebuild project:** Run `dotnet build`
2. **Clear bin/obj folders:** Delete and rebuild
3. **Check package installation:** Run `dotnet restore`
4. **Verify wwwroot:** Ensure static files are being served correctly

### Theme Not Loading

**Problem:** Chart displays without proper styling.

**Solutions:**
1. **Check index.html:** Ensure theme CSS is in `<head>` section
2. **Verify file path:** Path should be `_content/Syncfusion.Blazor.Themes/[theme].css`
3. **Check network tab:** Verify CSS file loads (F12 → Network → CSS)
4. **Match theme names:** Ensure Theme property matches CSS file name

### Serialization Issues

**Problem:** SaveChart() or LoadChartAsync() fails in WASM.

**Solutions:**
1. **Check for circular references:** WASM serialization is more strict
2. **Use JsonIgnore:** Mark properties that shouldn't serialize
3. **Test JSON validity:** Validate JSON string before loading
4. **Consider local storage:** Use browser localStorage for persistence:

```csharp
@inject IJSRuntime JS

await JS.InvokeVoidAsync("localStorage.setItem", "chartData", jsonString);
string? data = await JS.InvokeAsync<string>("localStorage.getItem", "chartData");
```

### Performance Issues

**Problem:** Chart interactions are slow or laggy.

**Solutions:**
1. **Reduce data size:** Limit number of data points
2. **Disable animations:** Set animation properties to false
3. **Use production build:** Run `dotnet publish -c Release`
4. **Profile with DevTools:** Identify performance bottlenecks
5. **Consider data virtualization:** For large datasets

## Next Steps

Now that you have a working Chart Wizard in your Blazor WASM app:

1. **Optimize bundle size:** Review dependencies and remove unused packages
2. **Add offline support:** Implement PWA features for offline capability
3. **Implement data persistence:** Use browser storage APIs for saving charts
4. **Add export features:** Configure export settings for saving charts
5. **Test across browsers:** Verify compatibility in Chrome, Firefox, Safari, Edge

For more advanced scenarios, explore the other guides for data binding, customization, export, and accessibility features.
