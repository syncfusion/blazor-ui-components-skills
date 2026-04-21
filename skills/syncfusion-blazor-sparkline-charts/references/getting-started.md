# Getting Started with Sparkline Charts

## Table of Contents
- [Installation](#installation)
    - [NuGet Package](#nuget-package)
    - [Prerequisites](#prerequisites)
- [Configuration](#configuration)
    - [Step 1: Import Namespaces](#step-1-import-namespaces)
    - [Step 2: Register Services](#step-2-register-services)
    - [Step 3: Add Script Reference](#step-3-add-script-reference)
    - [Step 4: Add Theme Stylesheet (Optional)](#step-4-add-theme-stylesheet-optional)
- [Basic Implementation](#basic-implementation)
    - [Minimal Sparkline](#minimal-sparkline)
    - [Sparkline with Object Data](#sparkline-with-object-data)
    - [First Complete Example](#first-complete-example)
- [Common Setup Issues](#common-setup-issues)
- [WebAssembly vs Server Considerations](#webassembly-vs-server-considerations)
- [Next Steps](#next-steps)
- [Quick Reference](#quick-reference)

This guide covers the essential steps to install and implement Syncfusion Blazor Sparkline Charts in your application.

## Installation

### NuGet Package

Install the `Syncfusion.Blazor.Sparkline` NuGet package:

**Visual Studio Package Manager:**
```powershell
Install-Package Syncfusion.Blazor.Sparkline
```

**.NET CLI:**
```bash
dotnet add package Syncfusion.Blazor.Sparkline
dotnet restore
```

### Prerequisites

- .NET SDK (latest version recommended)
- Blazor WebAssembly or Server project
- Visual Studio, VS Code, or any preferred IDE

## Configuration

### Step 1: Import Namespaces

Add the required namespaces to `_Imports.razor`:

```razor
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Charts
```

### Step 2: Register Services

Register Syncfusion Blazor services in `Program.cs`:

**Blazor WebAssembly:**
```csharp
using Microsoft.AspNetCore.Components.Web;
using Microsoft.AspNetCore.Components.WebAssembly.Hosting;
using Syncfusion.Blazor;

var builder = WebAssemblyHostBuilder.CreateDefault(args);
builder.RootComponents.Add<App>("#app");
builder.RootComponents.Add<HeadOutlet>("head::after");

builder.Services.AddScoped(sp => new HttpClient { 
    BaseAddress = new Uri(builder.HostEnvironment.BaseAddress) 
});

// Register Syncfusion Blazor service
builder.Services.AddSyncfusionBlazor();

await builder.Build().RunAsync();
```

**Blazor Server:**
```csharp
using Syncfusion.Blazor;

var builder = WebApplication.CreateBuilder(args);
builder.Services.AddRazorPages();
builder.Services.AddServerSideBlazor();

// Register Syncfusion Blazor service
builder.Services.AddSyncfusionBlazor();

var app = builder.Build();
// ... rest of configuration
```

### Step 3: Add Script Reference

Add the Syncfusion script in the `<head>` section of `index.html` (WebAssembly) or `_Host.cshtml` (Server):

```html
<head>
    <!-- ... other head content ... -->
    <script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js" type="text/javascript"></script>
</head>
```

### Step 4: Add Theme Stylesheet (Optional)

Include a Syncfusion theme for styled components:

```html
<head>
    <link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
</head>
```

**Available themes:** bootstrap5, material, fabric, tailwind, fluent, highcontrast

## Basic Implementation

### Minimal Sparkline

The simplest sparkline with just data:

```razor
@page "/sparkline-basic"
@using Syncfusion.Blazor.Charts

<h3>Basic Sparkline</h3>

<SfSparkline DataSource="@SimpleData" Height="50px" Width="200px">
</SfSparkline>

@code {
    public double[] SimpleData = new double[] { 3, 6, 4, 1, 3, 2, 5 };
}
```

**Result:** A line sparkline showing the data trend.

### Sparkline with Object Data

Using custom objects with field mapping:

```razor
@page "/sparkline-demo"
@using Syncfusion.Blazor.Charts

<h3>Monthly Sales Sparkline</h3>

<SfSparkline DataSource="@SalesData" 
              XName="Month" 
              YName="Sales"
              ValueType="SparklineValueType.Category"
              TValue="SalesInfo"
              Height="60px" 
              Width="250px">
</SfSparkline>

@code {
    public class SalesInfo
    {
        public string Month { get; set; }
        public double Sales { get; set; }
    }

    public List<SalesInfo> SalesData = new List<SalesInfo>
    {
        new SalesInfo { Month = "Jan", Sales = 35000 },
        new SalesInfo { Month = "Feb", Sales = 28000 },
        new SalesInfo { Month = "Mar", Sales = 34000 },
        new SalesInfo { Month = "Apr", Sales = 32000 },
        new SalesInfo { Month = "May", Sales = 40000 },
        new SalesInfo { Month = "Jun", Sales = 32000 }
    };
}
```

**Key Properties:**
- `DataSource` - Your data collection
- `XName` - Field name for X-axis values
- `YName` - Field name for Y-axis values
- `ValueType` - Data type: Numeric (default) or Category
- `TValue` - Generic type of your data model

### First Complete Example

A fully-configured sparkline with common features:

```razor
@page "/sparkline-complete"
@using Syncfusion.Blazor.Charts

<div class="sparkline-container">
    <h4>Temperature Trend (Last 12 Months)</h4>
    
    <SfSparkline DataSource="@TemperatureData"
                 TValue="WeatherReport"
                 XName="Month"
                 YName="Celsius"
                 ValueType="SparklineValueType.Category"
                 Type="SparklineType.Line"
                 Height="80px"
                 Width="300px"
                 Fill="#2196F3"
                 LineWidth="2">
        
        <SparklineMarkerSettings Visible="new List<VisibleType> { VisibleType.All }" 
                                 Size="5" 
                                 Fill="#ffffff"
                                 Border="new SparklineMarkerBorder { Color = \"#2196F3\", Width = 2 }">
        </SparklineMarkerSettings>
        
        <SparklineDataLabelSettings Visible="new List<VisibleType> { VisibleType.Start, VisibleType.End }">
        </SparklineDataLabelSettings>
        
        <SparklineTooltipSettings TValue="WeatherReport" 
                                  Visible="true"
                                  Format="${Month}: ${Celsius}°C">
        </SparklineTooltipSettings>
        
        <SparklinePadding Left="10" Right="10" Top="10" Bottom="10"></SparklinePadding>
        
    </SfSparkline>
</div>

@code {
    public class WeatherReport
    {
        public string Month { get; set; }
        public double Celsius { get; set; }
    }

    public List<WeatherReport> TemperatureData = new List<WeatherReport>
    {
        new WeatherReport { Month = "Jan", Celsius = 34 },
        new WeatherReport { Month = "Feb", Celsius = 36 },
        new WeatherReport { Month = "Mar", Celsius = 32 },
        new WeatherReport { Month = "Apr", Celsius = 35 },
        new WeatherReport { Month = "May", Celsius = 40 },
        new WeatherReport { Month = "Jun", Celsius = 38 },
        new WeatherReport { Month = "Jul", Celsius = 33 },
        new WeatherReport { Month = "Aug", Celsius = 37 },
        new WeatherReport { Month = "Sep", Celsius = 34 },
        new WeatherReport { Month = "Oct", Celsius = 31 },
        new WeatherReport { Month = "Nov", Celsius = 30 },
        new WeatherReport { Month = "Dec", Celsius = 29 }
    };
}
```

**This example includes:**
- Data binding with custom objects
- Markers on all data points
- Data labels on start and end points
- Tooltips with custom format
- Padding for visual spacing
- Custom colors and line width

## Common Setup Issues

### Issue: Component Not Rendering

**Symptoms:** Empty space where sparkline should appear, no errors

**Solutions:**
1. Verify `AddSyncfusionBlazor()` is registered in Program.cs
2. Check script reference in index.html/_Host.cshtml
3. Ensure namespaces are imported in _Imports.razor
4. Verify DataSource is not null or empty

### Issue: "Type or namespace name 'Syncfusion' could not be found"

**Symptoms:** Build errors, red squiggles in IDE

**Solutions:**
1. Verify NuGet package is installed: `Syncfusion.Blazor.Sparkline`
2. Run `dotnet restore` or rebuild solution
3. Check package version compatibility with your .NET version
4. Clear NuGet cache: `dotnet nuget locals all --clear`

### Issue: Data Not Displaying

**Symptoms:** Sparkline renders but shows no data

**Solutions:**
1. Ensure `XName` and `YName` match your data model property names (case-sensitive)
2. Set `ValueType` to `Category` if X-axis is string-based
3. Provide `TValue` generic type when using objects
4. Check data source has valid values (not all zeros or nulls)

### Issue: Script Load Errors

**Symptoms:** Console errors about "Syncfusion is not defined"

**Solutions:**
1. Verify script path: `_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js`
2. Check script is in `<head>` section, not `<body>`
3. Ensure Syncfusion.Blazor.Core package is installed
4. Clear browser cache and rebuild

## WebAssembly vs Server Considerations

### Blazor WebAssembly

**Characteristics:**
- Runs entirely in browser
- Initial load includes all assemblies
- No server connection after load

**Best practices:**
- Keep data sources reasonable size (<1000 points)
- Use async data loading for large datasets
- Consider data aggregation for performance

### Blazor Server

**Characteristics:**
- Maintains SignalR connection to server
- Smaller initial payload
- Real-time data updates easier

**Best practices:**
- Leverage server-side data processing
- Use StateHasChanged() for dynamic updates
- Handle connection interruptions gracefully

## Next Steps

After basic setup, explore these features:

1. **Chart Types:** [references/chart-types.md](chart-types.md) - Line, Area, Column, WinLoss, Pie
2. **Markers & Labels:** [references/markers-and-data-labels.md](markers-and-data-labels.md) - Visual data indicators
3. **Tooltips:** [references/user-interaction-and-events.md](user-interaction-and-events.md) - Interactive hover details
4. **Styling:** [references/dimensions-and-appearance.md](dimensions-and-appearance.md) - Colors, sizes, borders

## Quick Reference

**Essential Props:**
```razor
<SfSparkline DataSource="@data"          @* Required: Your data *@
              TValue="YourType"           @* Required for objects *@
              XName="PropName"            @* X-axis field *@
              YName="PropName"            @* Y-axis field *@
              ValueType="Category"        @* If X is string *@
              Type="Line"                 @* Chart type *@
              Height="50px"               @* Height *@
              Width="200px">              @* Width *@
</SfSparkline>
```

**Common Value Types:**
- `SparklineValueType.Numeric` (default)
- `SparklineValueType.Category`
- `SparklineValueType.DateTime`

You're now ready to implement sparklines in your Blazor application!
