# Getting Started with Smith Charts

## Table of Contents
- [Prerequisites](#prerequisites)
- [Installation](#installation)
    - [Step 1: Install NuGet Package](#step-1-install-nuget-package)
    - [Step 2: Register Syncfusion Services](#step-2-register-syncfusion-services)
    - [Step 3: Add Theme References](#step-3-add-theme-references)
    - [Step 4: Add Script Reference (Optional)](#step-4-add-script-reference-optional)
- [Your First Smith Chart](#your-first-smith-chart)
- [Multiple Series Example](#multiple-series-example)
- [Troubleshooting](#troubleshooting)
    - [Smith Chart Not Rendering](#smith-chart-not-rendering)
    - [License Key Warnings](#license-key-warnings)
    - [Data Not Displaying](#data-not-displaying)
    - [Theme Not Applied](#theme-not-applied)

This guide covers installation, configuration, and creating your first Smith Chart in Blazor Server, Blazor WebAssembly, and Blazor Web App projects.

## Prerequisites

Before starting, ensure your development environment meets:

- **Visual Studio 2022** (17.0 or later) or **Visual Studio Code** (latest)
- **.NET 6.0 SDK or later** (.NET 8.0 recommended)
- **Syncfusion Blazor License** (Trial or commercial license key)
- **Basic Blazor knowledge** (Components, data binding, Razor syntax)

## Installation

### Step 1: Install NuGet Package

Install the `Syncfusion.Blazor.SmithChart` package in your Blazor project.

**Package Manager Console:**
```bash
Install-Package Syncfusion.Blazor.SmithChart
```

**\.NET CLI:**
```bash
dotnet add package Syncfusion.Blazor.SmithChart
```

**Visual Studio NuGet Manager:**
1. Right-click project → **Manage NuGet Packages**
2. Search for **Syncfusion.Blazor.SmithChart**
3. Click **Install**

### Step 2: Register Syncfusion Services

#### Blazor Server (.NET 6/7/8)

Open `Program.cs` and add Syncfusion services:

```csharp
using Syncfusion.Blazor;

var builder = WebApplication.CreateBuilder(args);

// Add services to the container
builder.Services.AddRazorPages();
builder.Services.AddServerSideBlazor();

// Register Syncfusion Blazor service
builder.Services.AddSyncfusionBlazor();

var app = builder.Build();

// Configure the HTTP request pipeline
if (!app.Environment.IsDevelopment())
{
    app.UseExceptionHandler("/Error");
    app.UseHsts();
}

app.UseHttpsRedirection();
app.UseStaticFiles();
app.UseRouting();

app.MapBlazorHub();
app.MapFallbackToPage("/_Host");

app.Run();
```

**Register License Key:**

Add your license key before `AddSyncfusionBlazor()`:

```csharp
// Register Syncfusion license
Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense("YOUR_LICENSE_KEY");

builder.Services.AddSyncfusionBlazor();
```

#### Blazor WebAssembly (.NET 6/7/8)

Open `Program.cs`:

```csharp
using Microsoft.AspNetCore.Components.Web;
using Microsoft.AspNetCore.Components.WebAssembly.Hosting;
using Syncfusion.Blazor;
using YourAppNamespace;

var builder = WebAssemblyHostBuilder.CreateDefault(args);
builder.RootComponents.Add<App>("#app");
builder.RootComponents.Add<HeadOutlet>("head::after");

builder.Services.AddScoped(sp => new HttpClient 
{ 
    BaseAddress = new Uri(builder.HostEnvironment.BaseAddress) 
});

// Register Syncfusion license
Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense("YOUR_LICENSE_KEY");

// Register Syncfusion Blazor service
builder.Services.AddSyncfusionBlazor();

await builder.Build().RunAsync();
```

#### Blazor Web App (.NET 8+)

Open `Program.cs`:

```csharp
using Syncfusion.Blazor;

var builder = WebApplication.CreateBuilder(args);

// Add services to the container
builder.Services.AddRazorComponents()
    .AddInteractiveServerComponents()
    .AddInteractiveWebAssemblyComponents();

// Register Syncfusion license
Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense("YOUR_LICENSE_KEY");

// Register Syncfusion Blazor service
builder.Services.AddSyncfusionBlazor();

var app = builder.Build();

// Configure the HTTP request pipeline
if (app.Environment.IsDevelopment())
{
    app.UseWebAssemblyDebugging();
}
else
{
    app.UseExceptionHandler("/Error");
    app.UseHsts();
}

app.UseHttpsRedirection();
app.UseStaticFiles();
app.UseAntiforgery();

app.MapRazorComponents<App>()
    .AddInteractiveServerRenderMode()
    .AddInteractiveWebAssemblyRenderMode();

app.Run();
```

### Step 3: Add Theme References

Add Syncfusion theme CSS to your application.

#### Blazor Server

Open `Pages/_Host.cshtml` (or `_Layout.cshtml`) and add in `<head>`:

```html
<head>
    <!-- Other head content -->
    
    <!-- Syncfusion Blazor Theme -->
    <link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
</head>
```

#### Blazor WebAssembly

Open `wwwroot/index.html` and add in `<head>`:

```html
<head>
    <!-- Other head content -->
    
    <!-- Syncfusion Blazor Theme -->
    <link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
</head>
```

#### Blazor Web App (.NET 8)

Open `Components/App.razor` and add in `<head>`:

```razor
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <base href="/" />
    
    <!-- Syncfusion Blazor Theme -->
    <link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
    
    <link rel="stylesheet" href="app.css" />
    <HeadOutlet />
</head>
<body>
    <Routes />
    <script src="_framework/blazor.web.js"></script>
</body>
</html>
```

**Available Themes:**
- `bootstrap5.css` - Bootstrap 5 theme (recommended)
- `material.css` - Material Design theme
- `fabric.css` - Microsoft Fabric theme
- `tailwind.css` - Tailwind CSS theme
- `fluent.css` - Microsoft Fluent theme
- `highcontrast.css` - High contrast theme

### Step 4: Add Script Reference (Optional)

For advanced features like export, add the Syncfusion script (not required for basic charts):

```html
<script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js" type="text/javascript"></script>
```

## Your First Smith Chart

Create a new Razor page `SmithChartDemo.razor`:

```razor
@page "/smith-chart-demo"
@using Syncfusion.Blazor.Charts

<h3>Transmission Line Impedance Analysis</h3>

<SfSmithChart Height="600px" Width="100%">
    <SmithChartTitle Text="Impedance Matching Example"></SmithChartTitle>
    <SmithChartSeriesCollection>
        <SmithChartSeries Name="Transmission Line" 
                          DataSource="@TransmissionData" 
                          Reactance="Reactance" 
                          Resistance="Resistance">
            <SmithChartSeriesMarker Visible="true"></SmithChartSeriesMarker>
        </SmithChartSeries>
    </SmithChartSeriesCollection>
</SfSmithChart>

@code {
    public class SmithChartData
    {
        public double? Resistance { get; set; }
        public double? Reactance { get; set; }
    }
    
    public List<SmithChartData> TransmissionData = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 10, Reactance = 25 },
        new SmithChartData { Resistance = 8, Reactance = 10 },
        new SmithChartData { Resistance = 6, Reactance = 4.5 },
        new SmithChartData { Resistance = 4.5, Reactance = 2 },
        new SmithChartData { Resistance = 3.5, Reactance = 1.6 },
        new SmithChartData { Resistance = 2, Reactance = 1.2 },
        new SmithChartData { Resistance = 1, Reactance = 0.8 },
        new SmithChartData { Resistance = 0.5, Reactance = 0.4 },
        new SmithChartData { Resistance = 0, Reactance = 0.2 }
    };
}
```

## Multiple Series Example

Display multiple transmission lines for comparison:

```razor
@page "/multiple-series"
@using Syncfusion.Blazor.Charts

<h3>Antenna Tuning Comparison</h3>

<SfSmithChart Height="600px" Width="100%">
    <SmithChartTitle Text="Before and After Impedance Matching"></SmithChartTitle>
    <SmithChartLegendSettings Visible="true" Position="@Syncfusion.Blazor.Charts.LegendPosition.Bottom">
    </SmithChartLegendSettings>
    <SmithChartSeriesCollection>
        <SmithChartSeries Name="Before Matching" 
                          DataSource="@BeforeData" 
                          Reactance="Reactance" 
                          Resistance="Resistance"
                          Fill="#FF6B6B">
            <SmithChartSeriesMarker Visible="true" Shape="@Syncfusion.Blazor.Charts.Shape.Circle"></SmithChartSeriesMarker>
        </SmithChartSeries>
        <SmithChartSeries Name="After Matching" 
                          DataSource="@AfterData" 
                          Reactance="Reactance" 
                          Resistance="Resistance"
                          Fill="#4ECDC4">
            <SmithChartSeriesMarker Visible="true" Shape="@Syncfusion.Blazor.Charts.Shape.Diamond"></SmithChartSeriesMarker>
        </SmithChartSeries>
    </SmithChartSeriesCollection>
</SfSmithChart>

@code {
    public class SmithChartData
    {
        public double? Resistance { get; set; }
        public double? Reactance { get; set; }
    }
    
    public List<SmithChartData> BeforeData = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 10, Reactance = 25 },
        new SmithChartData { Resistance = 6, Reactance = 4.5 },
        new SmithChartData { Resistance = 3.5, Reactance = 1.6 },
        new SmithChartData { Resistance = 2, Reactance = 1.2 },
        new SmithChartData { Resistance = 1, Reactance = 0.8 },
        new SmithChartData { Resistance = 0, Reactance = 0.2 }
    };
    
    public List<SmithChartData> AfterData = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 5, Reactance = 2 },
        new SmithChartData { Resistance = 3, Reactance = 1.2 },
        new SmithChartData { Resistance = 2, Reactance = 0.8 },
        new SmithChartData { Resistance = 1.5, Reactance = 0.5 },
        new SmithChartData { Resistance = 1, Reactance = 0.2 },
        new SmithChartData { Resistance = 0.5, Reactance = 0.1 }
    };
}
```

## Troubleshooting

### Smith Chart Not Rendering

**Problem:** Component doesn't display or shows blank space.

**Solutions:**
1. Verify NuGet package is installed: `Syncfusion.Blazor.SmithChart`
2. Check theme CSS is referenced in HTML head
3. Ensure `AddSyncfusionBlazor()` is called in `Program.cs`
4. Verify license key is registered (check console for license warnings)
5. Confirm DataSource has valid data with Resistance and Reactance properties

### License Key Warnings

**Problem:** Console shows "License key not registered" warnings.

**Solutions:**
1. Register license key before `AddSyncfusionBlazor()`:
   ```csharp
   Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense("KEY");
   ```
2. Get license from [Syncfusion Portal](https://www.syncfusion.com/account/downloads)
3. For trials, register for a 30-day trial license

### Data Not Displaying

**Problem:** Smith Chart renders but series line is missing.

**Solutions:**
1. Verify DataSource is not null or empty
2. Check property names match: `Resistance="Resistance"` and `Reactance="Reactance"`
3. Ensure data properties use `double?` (nullable) not `double`
4. Verify data values are within reasonable ranges (0-∞ for normalized impedance)
5. Check browser console for JavaScript errors

### Theme Not Applied

**Problem:** Smith Chart appears unstyled or has incorrect colors.

**Solutions:**
1. Verify theme CSS link is in the correct location (`<head>` section)
2. Check path: `_content/Syncfusion.Blazor.Themes/bootstrap5.css`
3. Clear browser cache and rebuild project
4. Try a different theme to isolate the issue
5. Ensure static files middleware is configured: `app.UseStaticFiles()`

