# Getting Started with Blazor 3D Chart

Complete guide for installing, configuring, and creating your first Syncfusion Blazor 3D Chart component in Blazor Server, Blazor WebAssembly, and Blazor Web App (.NET 8+) projects.

## Table of Contents
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Configuration](#configuration)
- [Creating Your First 3D Chart](#creating-your-first-3d-chart)
- [Blazor Server Setup](#blazor-server-setup)
- [Blazor WebAssembly Setup](#blazor-webassembly-setup)
- [Blazor Web App Setup (.NET 8+)](#blazor-web-app-setup-net-8)
- [Render Mode Configuration](#render-mode-configuration)
- [Adding Features](#adding-features)
- [Troubleshooting](#troubleshooting)

## Prerequisites

**System Requirements:**
- .NET SDK 6.0, 7.0, or 8.0+
- Visual Studio 2022 (or Visual Studio Code with C# extension)
- Modern web browser (Chrome, Firefox, Edge, Safari)

**Knowledge Prerequisites:**
- Basic understanding of Blazor components
- Familiarity with C# and Razor syntax
- Understanding of data binding concepts

## Installation

### Step 1: Install NuGet Package

**Using Visual Studio:**
1. Right-click on project → Manage NuGet Packages
2. Search for `Syncfusion.Blazor.Chart3D`
3. Click Install

**Using Package Manager Console:**
```powershell
Install-Package Syncfusion.Blazor.Chart3D
```

**Using .NET CLI:**
```bash
dotnet add package Syncfusion.Blazor.Chart3D
dotnet restore
```

**Package Information:**
- **Package Name**: Syncfusion.Blazor.Chart3D
- **Namespace**: Syncfusion.Blazor.Chart3D
- **NuGet.org**: https://www.nuget.org/packages/Syncfusion.Blazor.Chart3D

### Step 2: Install Theme Package (Optional but Recommended)

For styled components, install the themes package:

```bash
dotnet add package Syncfusion.Blazor.Themes
```

## Configuration

### Step 1: Import Namespaces

Add to **_Imports.razor** file:

```razor
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Chart3D
```

This makes Syncfusion components available throughout your Blazor app without repeated using statements.

### Step 2: Register Syncfusion Service

**For Blazor Server (Program.cs):**
```csharp
using Syncfusion.Blazor;

var builder = WebApplication.CreateBuilder(args);

// Add services to the container
builder.Services.AddRazorPages();
builder.Services.AddServerSideBlazor();

// Register Syncfusion Blazor service
builder.Services.AddSyncfusionBlazor();

var app = builder.Build();
// ...rest of configuration
```

**For Blazor WebAssembly (Program.cs):**
```csharp
using Syncfusion.Blazor;
using Microsoft.AspNetCore.Components.Web;
using Microsoft.AspNetCore.Components.WebAssembly.Hosting;

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

**For Blazor Web App (.NET 8+):**
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
// ...rest of configuration
```

### Step 3: Add Script Reference

**For Blazor Server and Web App (.NET 8+):**

Add to **App.razor** or **_Layout.cshtml** at the end of `<body>`:

```html
<body>
    <!-- ... other content ... -->
    
    <script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js" 
            type="text/javascript"></script>
</body>
```

**For Blazor WebAssembly:**

Add to **index.html** in the `<head>` section:

```html
<head>
    <!-- ... other content ... -->
    
    <script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js" 
            type="text/javascript"></script>
</head>
```

### Step 4: Add Theme CSS (Optional)

Add theme CSS reference in the `<head>` section:

**For Blazor Server/Web App (App.razor or _Layout.cshtml):**
```html
<head>
    <link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
</head>
```

**For Blazor WebAssembly (index.html):**
```html
<head>
    <link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
</head>
```

**Available Themes:**
- `bootstrap5.css` - Bootstrap 5 theme
- `material.css` - Material Design
- `fluent.css` - Fluent UI
- `tailwind.css` - Tailwind CSS
- `fabric.css` - Office Fabric
- `material-dark.css` - Material Dark mode
- `bootstrap5-dark.css` - Bootstrap 5 Dark mode

## Creating Your First 3D Chart

### Minimal Example

Create a new Razor page or component (e.g., **Chart3DDemo.razor**):

```razor
@page "/chart3d-demo"
@using Syncfusion.Blazor.Chart3D

<h3>My First 3D Chart</h3>

<SfChart3D>
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@SalesData" 
                       XName="Month" 
                       YName="Sales" 
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class SalesInfo
    {
        public string Month { get; set; }
        public double Sales { get; set; }
    }

    public List<SalesInfo> SalesData = new List<SalesInfo>
    {
        new SalesInfo { Month = "Jan", Sales = 35 },
        new SalesInfo { Month = "Feb", Sales = 28 },
        new SalesInfo { Month = "Mar", Sales = 34 },
        new SalesInfo { Month = "Apr", Sales = 32 },
        new SalesInfo { Month = "May", Sales = 40 },
        new SalesInfo { Month = "Jun", Sales = 32 },
        new SalesInfo { Month = "Jul", Sales = 35 }
    };
}
```

### Complete Example with Features

```razor
@page "/chart3d-complete"
@using Syncfusion.Blazor.Chart3D

<h3>Sales Analysis Dashboard</h3>

<SfChart3D Title="Monthly Sales Report" 
           Width="100%" 
           Height="400px"
           WallColor="transparent" 
           EnableRotation="true" 
           RotationAngle="7" 
           TiltAngle="10" 
           Depth="100">
    
    <!-- X-Axis Configuration -->
    <Chart3DPrimaryXAxis Title="Month" 
                         ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <!-- Y-Axis Configuration -->
    <Chart3DPrimaryYAxis Title="Sales in Thousands ($)">
    </Chart3DPrimaryYAxis>
    
    <!-- Legend Configuration -->
    <Chart3DLegendSettings Visible="true" Position="Syncfusion.Blazor.Chart3D.LegendPosition.Bottom">
    </Chart3DLegendSettings>
    
    <!-- Tooltip Configuration -->
    <Chart3DTooltipSettings Enable="true" Format="${point.x}: ${point.y}K">
    </Chart3DTooltipSettings>
    
    <!-- Series Data -->
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@SalesData" 
                       Name="Product Sales" 
                       XName="Month" 
                       YName="Sales" 
                       Type="Chart3DSeriesType.Column"
                       Fill="#00bdae">
            <!-- Data Labels -->
            <Chart3DDataLabel Visible="true">
            </Chart3DDataLabel>
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class SalesInfo
    {
        public string Month { get; set; }
        public double Sales { get; set; }
    }

    public List<SalesInfo> SalesData = new List<SalesInfo>
    {
        new SalesInfo { Month = "Jan", Sales = 35 },
        new SalesInfo { Month = "Feb", Sales = 28 },
        new SalesInfo { Month = "Mar", Sales = 34 },
        new SalesInfo { Month = "Apr", Sales = 32 },
        new SalesInfo { Month = "May", Sales = 40 },
        new SalesInfo { Month = "Jun", Sales = 32 },
        new SalesInfo { Month = "Jul", Sales = 35 },
        new SalesInfo { Month = "Aug", Sales = 42 },
        new SalesInfo { Month = "Sep", Sales = 38 },
        new SalesInfo { Month = "Oct", Sales = 45 },
        new SalesInfo { Month = "Nov", Sales = 48 },
        new SalesInfo { Month = "Dec", Sales = 52 }
    };
}
```

## Blazor Server Setup

### Complete Setup Steps

1. **Create Blazor Server Project:**
```bash
dotnet new blazor -o MyChart3DApp -int Server
cd MyChart3DApp
```

2. **Install Package:**
```bash
dotnet add package Syncfusion.Blazor.Chart3D
```

3. **Configure Program.cs:**
```csharp
using Syncfusion.Blazor;

var builder = WebApplication.CreateBuilder(args);
builder.Services.AddRazorPages();
builder.Services.AddServerSideBlazor();
builder.Services.AddSyncfusionBlazor();

var app = builder.Build();
app.UseStaticFiles();
app.UseRouting();
app.MapBlazorHub();
app.MapFallbackToPage("/_Host");
app.Run();
```

4. **Add Imports (_Imports.razor):**
```razor
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Chart3D
```

5. **Add Script (App.razor or _Layout.cshtml):**
```html
<script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js"></script>
```

6. **Run Application:**
```bash
dotnet run
```

## Blazor WebAssembly Setup

### Complete Setup Steps

1. **Create Blazor WASM Project:**
```bash
dotnet new blazorwasm -o MyChart3DApp
cd MyChart3DApp
```

2. **Install Package:**
```bash
dotnet add package Syncfusion.Blazor.Chart3D
```

3. **Configure Program.cs:**
```csharp
using Syncfusion.Blazor;
using Microsoft.AspNetCore.Components.Web;
using Microsoft.AspNetCore.Components.WebAssembly.Hosting;

var builder = WebAssemblyHostBuilder.CreateDefault(args);
builder.RootComponents.Add<App>("#app");
builder.RootComponents.Add<HeadOutlet>("head::after");

builder.Services.AddScoped(sp => new HttpClient 
{ 
    BaseAddress = new Uri(builder.HostEnvironment.BaseAddress) 
});

builder.Services.AddSyncfusionBlazor();
await builder.Build().RunAsync();
```

4. **Add Imports (_Imports.razor):**
```razor
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Chart3D
```

5. **Add Script (index.html in wwwroot):**
```html
<head>
    <script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js"></script>
</head>
```

6. **Run Application:**
```bash
dotnet run
```

## Blazor Web App Setup (.NET 8+)

### Complete Setup Steps for .NET 8+ Projects

1. **Create Blazor Web App:**
```bash
dotnet new blazor -o MyChart3DApp
cd MyChart3DApp
```

2. **Install Package:**
```bash
dotnet add package Syncfusion.Blazor.Chart3D
```

3. **Configure Program.cs:**
```csharp
using Syncfusion.Blazor;

var builder = WebApplication.CreateBuilder(args);

// Add Blazor components with render modes
builder.Services.AddRazorComponents()
    .AddInteractiveServerComponents()
    .AddInteractiveWebAssemblyComponents();

// Register Syncfusion service
builder.Services.AddSyncfusionBlazor();

var app = builder.Build();

if (!app.Environment.IsDevelopment())
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

4. **Add Imports (_Imports.razor):**
```razor
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Chart3D
```

5. **Add Script (App.razor in Components folder):**
```html
<body>
    <script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js"></script>
</body>
```

## Render Mode Configuration

### .NET 8+ Render Modes

Blazor Web App (.NET 8+) supports multiple render modes for components.

**Option 1: Global Render Mode (in App.razor)**

Set globally for all pages:

```razor
<!DOCTYPE html>
<html>
<head>
    <!-- head content -->
</head>
<body>
    <Routes @rendermode="InteractiveServer" />
    <script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js"></script>
</body>
</html>
```

**Option 2: Per-Page Render Mode**

Set at the top of individual .razor pages:

```razor
@page "/chart3d"
@rendermode InteractiveServer

<SfChart3D>
    <!-- chart configuration -->
</SfChart3D>
```

**Available Render Modes:**
- **`InteractiveServer`**: Server-side with SignalR (real-time updates)
- **`InteractiveWebAssembly`**: Client-side execution in browser
- **`InteractiveAuto`**: Server initially, then WebAssembly after download

**When to Use Each Mode:**

| Mode | Best For | Pros | Cons |
|------|----------|------|------|
| InteractiveServer | Real-time dashboards, low latency needs | Fast initial load, low bandwidth | Requires constant connection |
| InteractiveWebAssembly | Offline capability, reduced server load | Works offline, no server connection | Larger initial download |
| InteractiveAuto | Best of both worlds | Fast start + offline later | More complex setup |

## Adding Features

### Adding Title and Axis Titles

```razor
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Sales Analysis">
    <Chart3DPrimaryXAxis Title="Month" 
                         ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DPrimaryYAxis Title="Sales in Dollars">
    </Chart3DPrimaryYAxis>
</SfChart3D>
```

### Enabling Legend

```razor
@using Syncfusion.Blazor.Chart3D

<SfChart3D>
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category"/>
    <Chart3DLegendSettings Visible="true" Position="Syncfusion.Blazor.Chart3D.LegendPosition.Right">
    </Chart3DLegendSettings>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries Name="Product A" DataSource="@Data1" XName="X" YName="Y" 
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
        <Chart3DSeries Name="Product B" DataSource="@Data2" XName="X" YName="Y" 
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class ChartPoint
    {
        public string X { get; set; }
        public double Y { get; set; }
    }

    public List<ChartPoint> Data1 = new List<ChartPoint>
    {
        new ChartPoint { X = "Q1", Y = 120 },
        new ChartPoint { X = "Q2", Y = 150 },
        new ChartPoint { X = "Q3", Y = 170 },
        new ChartPoint { X = "Q4", Y = 160 }
    };

    public List<ChartPoint> Data2 = new List<ChartPoint>
    {
        new ChartPoint { X = "Q1", Y = 100 },
        new ChartPoint { X = "Q2", Y = 140 },
        new ChartPoint { X = "Q3", Y = 155 },
        new ChartPoint { X = "Q4", Y = 180 }
    };
}
```

### Enabling Data Labels

```razor
@using Syncfusion.Blazor.Chart3D

<SfChart3D>
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category"/>
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@SalesData" XName="Month" YName="Sales" 
                       Type="Chart3DSeriesType.Column">
            <Chart3DDataLabel Visible="true">
            </Chart3DDataLabel>
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class SalesInfo
    {
        public string Month { get; set; }
        public double Sales { get; set; }
    }

    public List<SalesInfo> SalesData = new List<SalesInfo>
    {
        new SalesInfo { Month = "Jan", Sales = 12000 },
        new SalesInfo { Month = "Feb", Sales = 13500 },
        new SalesInfo { Month = "Mar", Sales = 15800 },
        new SalesInfo { Month = "Apr", Sales = 14900 },
        new SalesInfo { Month = "May", Sales = 17500 },
        new SalesInfo { Month = "Jun", Sales = 16200 },
        new SalesInfo { Month = "Jul", Sales = 18100 },
        new SalesInfo { Month = "Aug", Sales = 19000 },
        new SalesInfo { Month = "Sep", Sales = 16800 },
        new SalesInfo { Month = "Oct", Sales = 18500 },
        new SalesInfo { Month = "Nov", Sales = 19500 },
        new SalesInfo { Month = "Dec", Sales = 21000 }
    };
}
```

### Enabling Tooltip

```razor
@using Syncfusion.Blazor.Chart3D

<SfChart3D>
    <Chart3DTooltipSettings Enable="true" Format="${point.x}: ${point.y}">
    </Chart3DTooltipSettings>
    
    <Chart3DSeriesCollection>
        <!-- series configuration -->
    </Chart3DSeriesCollection>
</SfChart3D>
```

### Adding 3D Effects

```razor
@using Syncfusion.Blazor.Chart3D

<SfChart3D EnableRotation="true" 
           RotationAngle="15" 
           TiltAngle="20" 
           Depth="120"
           WallColor="#f0f0f0">
    <!-- chart configuration -->
</SfChart3D>
```

## Troubleshooting

### Chart Not Rendering

**Problem**: Blank space where chart should appear

**Solutions:**
1. Verify service registration:
   ```csharp
   builder.Services.AddSyncfusionBlazor();
   ```

2. Check namespace imports in _Imports.razor:
   ```razor
   @using Syncfusion.Blazor.Chart3D
   ```

3. Ensure script reference is added:
   ```html
   <script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js"></script>
   ```

4. For .NET 8+, verify render mode is set:
   ```razor
   @rendermode InteractiveServer
   ```

### Data Not Displaying

**Problem**: Chart renders but shows no data

**Solutions:**
1. Check XName and YName match property names exactly (case-sensitive):
   ```razor
   <Chart3DSeries DataSource="@Sales" XName="Month" YName="SalesValue">
   ```
   Properties must be named `Month` and `SalesValue` in your class.

2. Verify DataSource is not null and contains data:
   ```csharp
   protected override void OnInitialized()
   {
       if (SalesData == null || !SalesData.Any())
       {
           // Initialize data
       }
   }
   ```

3. Ensure ValueType matches data:
   - Use `ValueType.Category` for string X-axis
   - Use `ValueType.Numeric` or `ValueType.Double` for numeric X-axis

### Script Loading Errors

**Problem**: Console errors about missing Syncfusion scripts

**Solutions:**
1. Verify NuGet package is installed:
   ```bash
   dotnet list package | grep Syncfusion.Blazor.Chart3D
   ```

2. Clean and rebuild:
   ```bash
   dotnet clean
   dotnet build
   ```

3. Check script path matches your project structure:
   - Blazor Server/Web App: In App.razor `<body>`
   - Blazor WASM: In wwwroot/index.html `<head>`

### License Warning

**Problem**: License warning message appearing

**Solution**: Register Syncfusion license key in Program.cs before `builder.Build()`:

```csharp
Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense("YOUR-LICENSE-KEY");
var app = builder.Build();
```

Get a free community license or trial key from https://www.syncfusion.com/sales/products

### Performance Issues

**Problem**: Chart rendering slowly or browser freezing

**Solutions:**
1. Limit data points (recommended < 1000 for 3D charts)
2. Disable rotation if not needed: `EnableRotation="false"`
3. Reduce depth value: `Depth="50"` instead of `Depth="200"`
4. Use pagination or data virtualization for large datasets

## Next Steps

After setting up your first 3D chart, explore:
- Different chart types (Column, Bar, Stacked Column, Stacked Bar)
- Data binding options (IEnumerable, remote data, dynamic objects)
- Axis configuration (Category, Numeric, DateTime, Logarithmic)
- Customization options (colors, styles, themes)
- Advanced features (selection, export, multiple panes)

Your 3D Chart is now ready for further customization and feature additions!
