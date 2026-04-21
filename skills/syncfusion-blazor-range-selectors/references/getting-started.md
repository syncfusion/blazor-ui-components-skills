# Getting Started with Range Selector

This guide covers installation, setup, and creating your first Syncfusion Blazor Range Selector (RangeNavigator) component.

## API Quick Reference

**Main Component:** `SfRangeNavigator` (Namespace: `Syncfusion.Blazor.Charts`)

**Essential Properties:**
- `Value` - Selected range as array (DateTime[] or double[])
- `ValueType` - Data type: RangeValueType.DateTime, Double, or Logarithmic
- `DataSource` - Data collection for series
- `Width` - Component width (default: "100%")
- `Height` - Component height (default: "80px")

**Key Child Components:**
- `RangeNavigatorSeriesCollection` - Container for series
- `RangeNavigatorSeries` - Individual series configuration
- `RangeNavigatorRangeTooltipSettings` - Tooltip settings
- `RangeNavigatorPeriodSelectorSettings` - Period selector configuration

For complete API reference, see [api-reference.md](api-reference.md).

## Table of Contents

- [API Quick Reference](#api-quick-reference)
- [Installation](#installation)
    - [NuGet Package](#nuget-package)
- [Project Setup](#project-setup)
    - [Blazor Server (.NET 6.0+)](#blazor-server-net-60)
    - [Blazor WebAssembly (.NET 6.0+)](#blazor-webassembly-net-60)
    - [Blazor Web App (.NET 8.0+)](#blazor-web-app-net-80)
- [Your First Range Selector](#your-first-range-selector)
    - [Basic Implementation](#basic-implementation)
    - [Component Structure](#component-structure)
    - [Key Properties Explained](#key-properties-explained)
- [Available Themes](#available-themes)
- [Troubleshooting](#troubleshooting)
    - [Issue: "Syncfusion.Blazor.Charts namespace could not be found"](#issue-syncfusionblazorcharts-namespace-could-not-be-found)
    - [Issue: Component not rendering or showing blank](#issue-component-not-rendering-or-showing-blank)
    - [Issue: "Cannot find 'Syncfusion.Blazor.Core' scripts"](#issue-cannot-find-syncfusionblazorcore-scripts)
    - [Issue: Thumbs not draggable or selection not working](#issue-thumbs-not-draggable-or-selection-not-working)
    - [Issue: Labels not displaying correctly](#issue-labels-not-displaying-correctly)
    - [Issue: Performance issues with large datasets](#issue-performance-issues-with-large-datasets)
- [Additional Resources](#additional-resources)

## Installation

### NuGet Package

Install the Range Selector package via NuGet Package Manager, .NET CLI, or Package Manager Console.

**Package Manager:**
```powershell
Install-Package Syncfusion.Blazor.Charts
```

**NET CLI:**
```bash
dotnet add package Syncfusion.Blazor.Charts
```

**Package Reference (in .csproj):**
```xml
<ItemGroup>
  <PackageReference Include="Syncfusion.Blazor.Charts" Version="25.*.*" />
  <PackageReference Include="Syncfusion.Blazor.Themes" Version="25.*.*" />
</ItemGroup>
```

> **Note**: The Range Selector is part of the `Syncfusion.Blazor.Charts` package, which also includes Stock Chart and other charting components.

## Project Setup

### Blazor Server (.NET 6.0+)

**1. Register Syncfusion Services**

In `Program.cs`:

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

**2. Add Theme Reference**

In `Pages/_Layout.cshtml` (or `_Host.cshtml` for older projects):

```html
<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>MyBlazorApp</title>
    <base href="~/" />
    
    <!-- Syncfusion Blazor Theme -->
    <link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
    
    <link href="css/site.css" rel="stylesheet" />
</head>
```

**3. Add Script Reference**

Add before the closing `</body>` tag in `_Layout.cshtml`:

```html
    <script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js" type="text/javascript"></script>
</body>
```

### Blazor WebAssembly (.NET 6.0+)

**1. Register Syncfusion Services**

In `Program.cs`:

```csharp
using Microsoft.AspNetCore.Components.Web;
using Microsoft.AspNetCore.Components.WebAssembly.Hosting;
using Syncfusion.Blazor;
using MyBlazorWasmApp;

var builder = WebAssemblyHostBuilder.CreateDefault(args);
builder.RootComponents.Add<App>("#app");
builder.RootComponents.Add<HeadOutlet>("head::after");

builder.Services.AddScoped(sp => new HttpClient { BaseAddress = new Uri(builder.HostEnvironment.BaseAddress) });

// Register Syncfusion Blazor service
builder.Services.AddSyncfusionBlazor();

await builder.Build().RunAsync();
```

**2. Add Theme and Script References**

In `wwwroot/index.html`:

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>MyBlazorWasmApp</title>
    <base href="/" />
    
    <!-- Syncfusion Blazor Theme -->
    <link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
    
    <link href="css/app.css" rel="stylesheet" />
</head>
<body>
    <div id="app">Loading...</div>
    
    <script src="_framework/blazor.webassembly.js"></script>
    <script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js" type="text/javascript"></script>
</body>
</html>
```

### Blazor Web App (.NET 8.0+)

**1. Register Syncfusion Services**

In `Program.cs`:

```csharp
using Syncfusion.Blazor;

var builder = WebApplication.CreateBuilder(args);

// Add services to the container
builder.Services.AddRazorComponents()
    .AddInteractiveServerComponents()
    .AddInteractiveWebAssemblyComponents();

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

**2. Add Theme and Script References**

In `Components/App.razor`:

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
    <script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js" type="text/javascript"></script>
</body>
</html>
```

## Your First Range Selector

### Basic Implementation

Create a new Razor component (`RangeSelectorDemo.razor`) with a simple range selector:

```razor
@page "/rangeselector-demo"
@using Syncfusion.Blazor.Charts

<h3>Stock Price Range Selector</h3>

<SfRangeNavigator Value="@SelectedRange" 
                  ValueType="RangeValueType.DateTime"
                  LabelFormat="MMM-yy"
                  IntervalType="RangeIntervalType.Months">
    <RangeNavigatorRangeTooltipSettings Enable="true"></RangeNavigatorRangeTooltipSettings>
    <RangeNavigatorSeriesCollection>
        <RangeNavigatorSeries DataSource="@StockData" 
                              XName="Date" 
                              YName="Close" 
                              Type="RangeNavigatorType.Area">
        </RangeNavigatorSeries>
    </RangeNavigatorSeriesCollection>
</SfRangeNavigator>

<div style="margin-top: 20px;">
    <p><strong>Selected Range:</strong></p>
    <p>Start: @SelectedRange[0].ToShortDateString()</p>
    <p>End: @SelectedRange[1].ToShortDateString()</p>
</div>

@code {
    public class StockInfo
    {
        public DateTime Date { get; set; }
        public double Close { get; set; }
    }
    
    public DateTime[] SelectedRange = new DateTime[] 
    { 
        new DateTime(2021, 01, 01), 
        new DateTime(2022, 01, 01) 
    };
    
    public List<StockInfo> StockData = new List<StockInfo>
    {
        new StockInfo { Date = new DateTime(2018, 01, 01), Close = 35 },
        new StockInfo { Date = new DateTime(2018, 06, 01), Close = 38 },
        new StockInfo { Date = new DateTime(2019, 01, 01), Close = 42 },
        new StockInfo { Date = new DateTime(2019, 06, 01), Close = 45 },
        new StockInfo { Date = new DateTime(2020, 01, 01), Close = 48 },
        new StockInfo { Date = new DateTime(2020, 06, 01), Close = 52 },
        new StockInfo { Date = new DateTime(2021, 01, 01), Close = 56 },
        new StockInfo { Date = new DateTime(2021, 06, 01), Close = 59 },
        new StockInfo { Date = new DateTime(2022, 01, 01), Close = 62 },
        new StockInfo { Date = new DateTime(2022, 06, 01), Close = 65 },
        new StockInfo { Date = new DateTime(2023, 01, 01), Close = 70 }
    };
}
```

**What this creates:**
- Area chart showing stock price trend from 2018-2023
- Draggable thumbs for selecting date range
- Initial selection: January 2021 to January 2022
- Month labels formatted as "MMM-yy" (e.g., "Jan-21")
- Tooltip displaying values on hover
- Selected range displayed below the selector

### Component Structure

The Range Selector consists of:

1. **SfRangeNavigator**: Main component container
2. **RangeNavigatorRangeTooltipSettings**: Tooltip configuration
3. **RangeNavigatorSeriesCollection**: Collection of data series
4. **RangeNavigatorSeries**: Individual data series with type and data binding

### Key Properties Explained

- **Value**: Array containing start and end values `[start, end]`
- **ValueType**: Data type (DateTime, Double, Logarithmic)
- **LabelFormat**: Display format for axis labels
- **IntervalType**: Interval between labels (Auto, Years, Months, Days, etc.)
- **DataSource**: Data collection for the series
- **XName**: Property name for x-axis values
- **YName**: Property name for y-axis values
- **Type**: Series visualization type (Line, Area, StepLine)

## Available Themes

Syncfusion Blazor provides multiple built-in themes. Change the theme by modifying the stylesheet reference:

```html
<!-- Material Theme (Default) -->
<link href="_content/Syncfusion.Blazor.Themes/material.css" rel="stylesheet" />

<!-- Bootstrap 5 Theme -->
<link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />

<!-- Fluent Theme -->
<link href="_content/Syncfusion.Blazor.Themes/fluent.css" rel="stylesheet" />

<!-- Tailwind Theme -->
<link href="_content/Syncfusion.Blazor.Themes/tailwind.css" rel="stylesheet" />

<!-- Fabric Theme -->
<link href="_content/Syncfusion.Blazor.Themes/fabric.css" rel="stylesheet" />

<!-- High Contrast Theme -->
<link href="_content/Syncfusion.Blazor.Themes/highcontrast.css" rel="stylesheet" />
```

## Troubleshooting

### Issue: "Syncfusion.Blazor.Charts namespace could not be found"

**Solution**: Ensure you've installed the correct NuGet package:
```bash
dotnet add package Syncfusion.Blazor.Charts
```

Add the using statement in your component or `_Imports.razor`:
```razor
@using Syncfusion.Blazor.Charts
```

### Issue: Component not rendering or showing blank

**Solution**: Verify all setup steps:
1. ✅ NuGet package installed
2. ✅ Service registered (`AddSyncfusionBlazor()`)
3. ✅ Theme stylesheet referenced
4. ✅ Syncfusion script referenced
5. ✅ DataSource has valid data
6. ✅ XName and YName match data property names

### Issue: "Cannot find 'Syncfusion.Blazor.Core' scripts"

**Solution**: Ensure the script reference path is correct and the package is installed:
```bash
dotnet add package Syncfusion.Blazor.Core
```

Script reference:
```html
<script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js" type="text/javascript"></script>
```

### Issue: Thumbs not draggable or selection not working

**Solution**: Check that:
- `Value` property is bound correctly (use `@bind-Value` for two-way binding)
- `ValueType` matches your data type (DateTime for dates, Double for numbers)
- Data is sorted in ascending order by X values
- XName and YName properties are correctly set

### Issue: Labels not displaying correctly

**Solution**: 
- Set `LabelFormat` property for custom formatting
- Ensure `IntervalType` matches your data (use `RangeIntervalType.Months` for monthly data)
- Check that data spans the expected range

### Issue: Performance issues with large datasets

**Solution**: Enable grouping for better performance:
```razor
<SfRangeNavigator EnableGrouping="true" 
                  GroupBy="RangeIntervalType.Months">
```

## Additional Resources

- **Syncfusion Documentation**: [Range Navigator Overview](https://help.syncfusion.com/blazor/range-navigator/overview)
- **API Reference**: [RangeNavigator API](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.Charts.SfRangeNavigator.html)
- **Live Demos**: [Range Navigator Demos](https://blazor.syncfusion.com/demos/range-navigator/default)
