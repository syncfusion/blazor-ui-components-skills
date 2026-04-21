# Getting Started with Stock Charts

This guide walks you through installing and setting up the Syncfusion Blazor Stock Chart component in your Blazor application. Follow these steps to create your first stock chart.

## Table of Contents
- [Prerequisites](#prerequisites)
- [Installation Steps](#installation-steps)
- [Create Your First Stock Chart](#create-your-first-stock-chart)
- [Data Structure Requirements](#data-structure-requirements)
- [Common Setup Issues](#common-setup-issues)
- [Next Steps](#next-steps)
- [Quick Reference](#quick-reference)

## Prerequisites

Before you begin, ensure you have:
- Visual Studio 2022 (or later), Visual Studio Code, or .NET CLI
- .NET 6.0 SDK or later
- Basic understanding of Blazor application structure
- A Blazor Server, WebAssembly, or Web App project

Check the [Syncfusion Blazor system requirements](https://blazor.syncfusion.com/documentation/system-requirements) for detailed prerequisites.

## Installation Steps

### Step 1: Install NuGet Packages

The Stock Chart requires two NuGet packages:

**Using Visual Studio:**
1. Open NuGet Package Manager (*Tools → NuGet Package Manager → Manage NuGet Packages for Solution*)
2. Search for and install:
   - `Syncfusion.Blazor.StockChart` (for the component)
   - `Syncfusion.Blazor.Themes` (for styling)

**Using .NET CLI:**
```bash
dotnet add package Syncfusion.Blazor.StockChart
dotnet add package Syncfusion.Blazor.Themes
dotnet restore
```

**Using Package Manager Console:**
```powershell
Install-Package Syncfusion.Blazor.StockChart
Install-Package Syncfusion.Blazor.Themes
```

### Step 2: Import Namespaces

Add the required namespaces to `_Imports.razor`:

```razor
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Charts
```

This makes the Stock Chart component and related types available throughout your application.

### Step 3: Register Syncfusion Service

Register the Syncfusion Blazor service in `Program.cs`:

**Blazor WebAssembly:**
```csharp
using Syncfusion.Blazor;

var builder = WebAssemblyHostBuilder.CreateDefault(args);
builder.RootComponents.Add<App>("#app");
builder.RootComponents.Add<HeadOutlet>("head::after");

builder.Services.AddScoped(sp => new HttpClient { BaseAddress = new Uri(builder.HostEnvironment.BaseAddress) });
builder.Services.AddSyncfusionBlazor();

await builder.Build().RunAsync();
```

**Blazor Server:**
```csharp
using Syncfusion.Blazor;

var builder = WebApplication.CreateBuilder(args);
builder.Services.AddRazorPages();
builder.Services.AddServerSideBlazor();
builder.Services.AddSyncfusionBlazor();

var app = builder.Build();
// ... rest of configuration
```

**Blazor Web App (.NET 8+):**
```csharp
using Syncfusion.Blazor;

var builder = WebApplication.CreateBuilder(args);
builder.Services.AddRazorComponents()
    .AddInteractiveServerComponents()
    .AddInteractiveWebAssemblyComponents();

builder.Services.AddSyncfusionBlazor();

var app = builder.Build();
// ... rest of configuration
```

### Step 4: Add Stylesheet and Script References

Add theme stylesheet and script to the `<head>` section:

**Blazor WebAssembly (`wwwroot/index.html`):**
```html
<head>
    <meta charset="utf-8" />
    <title>My Stock Chart App</title>
    <link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
    <script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js" type="text/javascript"></script>
</head>
```

**Blazor Server (`Pages/_Host.cshtml` or `_Layout.cshtml`):**
```html
<head>
    <meta charset="utf-8" />
    <title>My Stock Chart App</title>
    <link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
    <script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js" type="text/javascript"></script>
</head>
```

**Blazor Web App (`Components/App.razor`):**
```html
<head>
    <meta charset="utf-8" />
    <title>My Stock Chart App</title>
    <link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
    <script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js" type="text/javascript"></script>
</head>
```

**Available Themes:**
- `bootstrap5.css` - Bootstrap 5 (default)
- `material.css` - Material Design
- `fluent.css` - Microsoft Fluent UI
- `tailwind.css` - Tailwind CSS
- `fabric.css` - Office Fabric
- Add `-dark` suffix for dark mode (e.g., `material-dark.css`)

## Create Your First Stock Chart

### Basic Stock Chart Implementation

Create a new Razor page (e.g., `Pages/StockChart.razor`) and add this minimal implementation:

```razor
@page "/stock-chart"
@using Syncfusion.Blazor.Charts

<h3>Stock Price Chart</h3>

<SfStockChart>
    <StockChartSeriesCollection>
        <StockChartSeries DataSource="@StockData" 
                          Type="ChartSeriesType.Candle" 
                          XName="Date" 
                          High="High" 
                          Low="Low" 
                          Open="Open" 
                          Close="Close">
        </StockChartSeries>
    </StockChartSeriesCollection>
</SfStockChart>

@code {
    public class StockInfo
    {
        public DateTime Date { get; set; }
        public double Open { get; set; }
        public double High { get; set; }
        public double Low { get; set; }
        public double Close { get; set; }
        public double Volume { get; set; }
    }

    public List<StockInfo> StockData = new List<StockInfo>
    {
        new StockInfo { Date = new DateTime(2024, 01, 01), Open = 175.5, High = 180.2, Low = 174.8, Close = 179.5, Volume = 12000000 },
        new StockInfo { Date = new DateTime(2024, 01, 08), Open = 179.5, High = 185.3, Low = 178.1, Close = 183.7, Volume = 15000000 },
        new StockInfo { Date = new DateTime(2024, 01, 15), Open = 183.7, High = 190.5, Low = 182.5, Close = 188.2, Volume = 18000000 },
        new StockInfo { Date = new DateTime(2024, 01, 22), Open = 188.2, High = 193.8, Low = 186.4, Close = 191.3, Volume = 16000000 },
        new StockInfo { Date = new DateTime(2024, 01, 29), Open = 191.3, High = 195.7, Low = 189.2, Close = 193.5, Volume = 14000000 },
        new StockInfo { Date = new DateTime(2024, 02, 05), Open = 193.5, High = 198.4, Low = 192.1, Close = 196.8, Volume = 17000000 },
        new StockInfo { Date = new DateTime(2024, 02, 12), Open = 196.8, High = 201.2, Low = 195.3, Close = 199.7, Volume = 19000000 },
        new StockInfo { Date = new DateTime(2024, 02, 19), Open = 199.7, High = 205.1, Low = 198.5, Close = 203.4, Volume = 21000000 }
    };
}
```

Run your application (`Ctrl+F5` or `dotnet run`) and navigate to `/stock-chart` to see your first stock chart!

### Adding a Title

Make your chart more informative by adding a title:

```razor
<SfStockChart Title="AAPL Stock Price - 2024">
    <StockChartSeriesCollection>
        <StockChartSeries DataSource="@StockData" Type="ChartSeriesType.Candle" 
                          XName="Date" High="High" Low="Low" Open="Open" Close="Close">
        </StockChartSeries>
    </StockChartSeriesCollection>
</SfStockChart>
```

### Adding Period Selector

Enable time-based navigation with preset period buttons:

```razor
<SfStockChart Title="AAPL Stock Price">
    <StockChartPrimaryXAxis ValueType="Syncfusion.Blazor.Charts.ValueType.DateTime">
    </StockChartPrimaryXAxis>
    
    <StockChartSeriesCollection>
        <StockChartSeries DataSource="@StockData" Type="ChartSeriesType.Candle" 
                          XName="Date" High="High" Low="Low" Open="Open" Close="Close">
        </StockChartSeries>
    </StockChartSeriesCollection>
    
    <StockChartPeriods>
        <StockChartPeriod Text="1M" Interval="1" IntervalType="RangeIntervalType.Months"></StockChartPeriod>
        <StockChartPeriod Text="3M" Interval="3" IntervalType="RangeIntervalType.Months"></StockChartPeriod>
        <StockChartPeriod Text="6M" Interval="6" IntervalType="RangeIntervalType.Months"></StockChartPeriod>
        <StockChartPeriod Text="1Y" Interval="1" IntervalType="RangeIntervalType.Years"></StockChartPeriod>
        <StockChartPeriod Text="All"></StockChartPeriod>
    </StockChartPeriods>
</SfStockChart>
```

### Adding Crosshair and Tooltip

Enable interactive data exploration:

```razor
<SfStockChart Title="AAPL Stock Price">
    <StockChartCrosshairSettings Enable="true"></StockChartCrosshairSettings>
    <StockChartTooltipSettings Enable="true"></StockChartTooltipSettings>
    
    <StockChartSeriesCollection>
        <StockChartSeries DataSource="@StockData" Type="ChartSeriesType.Candle" 
                          XName="Date" High="High" Low="Low" Open="Open" Close="Close">
        </StockChartSeries>
    </StockChartSeriesCollection>
</SfStockChart>
```

## Data Structure Requirements

### OHLC Data Format

Stock charts typically use OHLC (Open, High, Low, Close) data structure:

```csharp
public class StockInfo
{
    public DateTime Date { get; set; }     // Required: X-axis value
    public double Open { get; set; }       // Opening price
    public double High { get; set; }       // Highest price
    public double Low { get; set; }        // Lowest price
    public double Close { get; set; }      // Closing price
    public double Volume { get; set; }     // Optional: Trading volume
}
```

**Property mapping:**
- `XName="Date"` - Maps to the Date property (X-axis)
- `Open="Open"` - Maps to opening price
- `High="High"` - Maps to high price
- `Low="Low"` - Maps to low price
- `Close="Close"` - Maps to closing price
- `Volume="Volume"` - Maps to volume (optional)

### Minimal Data Requirements

Different series types require different data fields:

| Series Type | Required Fields |
|-------------|----------------|
| **Candle** | Date, Open, High, Low, Close |
| **HiloOpenClose** | Date, Open, High, Low, Close |
| **Hilo** | Date, High, Low |
| **Line/Spline** | Date, Close (or any single Y value) |

## Common Setup Issues

### Issue: Chart not rendering

**Symptoms:** Empty space where chart should appear, no errors in console

**Solutions:**
1. Verify script reference is in `<head>` section
2. Check that `AddSyncfusionBlazor()` is called in `Program.cs`
3. Ensure namespaces are imported in `_Imports.razor`
4. Confirm DataSource has data and property names match XName/High/Low/Open/Close

### Issue: Theme not applying

**Symptoms:** Chart renders but looks unstyled or has default black/white colors

**Solutions:**
1. Verify theme CSS link is before script reference in `<head>`
2. Check the theme file path is correct (should start with `_content/Syncfusion.Blazor.Themes/`)
3. Clear browser cache and rebuild solution
4. Ensure `Syncfusion.Blazor.Themes` NuGet package is installed

### Issue: "AddSyncfusionBlazor is not recognized"

**Symptoms:** Compilation error in Program.cs

**Solutions:**
1. Add `using Syncfusion.Blazor;` at top of Program.cs
2. Ensure `Syncfusion.Blazor.Core` package is installed (dependency of StockChart)
3. Run `dotnet restore` to refresh packages

### Issue: Data not displaying

**Symptoms:** Chart renders but shows no data points

**Solutions:**
1. Verify property names in data class match `XName`, `High`, `Low`, `Open`, `Close` values
2. Check that Date values are valid DateTime objects
3. Ensure OHLC values are numeric (double/decimal)
4. Add at least 3-5 data points for meaningful visualization

## Next Steps

Now that you have a basic stock chart running:

1. **Explore series types** - Try different visualizations (Line, Spline, Candle) - see [Series Types](series-types.md)
2. **Bind real data** - Connect to APIs or databases - see [Working with Data](working-with-data.md)
3. **Add technical indicators** - Include SMA, EMA, RSI - see [Technical Indicators](technical-indicators.md)
4. **Customize appearance** - Change colors, themes, dimensions - see [Appearance Customization](appearance-customization.md)
5. **Enable zooming** - Allow users to explore data - see [Zooming, Panning, and Crosshair](zooming-panning-crosshair.md)

## Quick Reference

### Minimal Required Setup

```razor
@page "/stock"
@using Syncfusion.Blazor.Charts

<SfStockChart>
    <StockChartSeriesCollection>
        <StockChartSeries DataSource="@Data" XName="X" High="High" Low="Low" Open="Open" Close="Close">
        </StockChartSeries>
    </StockChartSeriesCollection>
</SfStockChart>

@code {
    public List<OhlcData> Data = new List<OhlcData> { /* your data */ };
}
```

### Required Files Checklist

- [ ] `Syncfusion.Blazor.StockChart` NuGet package installed
- [ ] `Syncfusion.Blazor.Themes` NuGet package installed
- [ ] Namespaces added to `_Imports.razor`
- [ ] `AddSyncfusionBlazor()` called in `Program.cs`
- [ ] Theme CSS link in `<head>`
- [ ] Script reference in `<head>`
- [ ] Data class with DateTime and OHLC properties
- [ ] DataSource bound to series

You're now ready to build sophisticated financial data visualizations with Syncfusion Blazor Stock Chart!
