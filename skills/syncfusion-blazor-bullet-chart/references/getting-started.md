# Getting Started with Bullet Charts

This guide walks you through installing and setting up the Syncfusion Blazor Bullet Chart component in your Blazor application. Follow these steps to create your first bullet chart for KPI visualization.

## Table of Contents
- [Prerequisites](#prerequisites)
- [Installation Steps](#installation-steps)
- [Your First Bullet Chart](#your-first-bullet-chart)
- [Project Structure](#project-structure)
- [Troubleshooting](#troubleshooting)

## Prerequisites

Before you begin, ensure you have:
- Visual Studio 2022 (or later), Visual Studio Code, or .NET CLI
- .NET 6.0 SDK or later
- Basic understanding of Blazor application structure
- A Blazor Server, WebAssembly, or Web App project

Check the [Syncfusion Blazor system requirements](https://blazor.syncfusion.com/documentation/system-requirements) for detailed prerequisites.

## Installation Steps

### Step 1: Install NuGet Packages

The Bullet Chart requires two NuGet packages:

**Using Visual Studio:**
1. Open NuGet Package Manager (*Tools → NuGet Package Manager → Manage NuGet Packages for Solution*)
2. Search for and install:
   - `Syncfusion.Blazor.BulletChart` (for the component)
   - `Syncfusion.Blazor.Themes` (for styling)

**Using .NET CLI:**
```bash
dotnet add package Syncfusion.Blazor.BulletChart
dotnet add package Syncfusion.Blazor.Themes
dotnet restore
```

**Using Package Manager Console:**
```powershell
Install-Package Syncfusion.Blazor.BulletChart
Install-Package Syncfusion.Blazor.Themes
```

### Step 2: Import Namespaces

Add the required namespaces to `_Imports.razor`:

```razor
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Charts
```

This makes the Bullet Chart component and related types available throughout your application.

### Step 3: Register Syncfusion Service

Register the Syncfusion Blazor service in your application's startup file.

**For Blazor Server (Program.cs or Startup.cs):**

```csharp
using Syncfusion.Blazor;

var builder = WebApplication.CreateBuilder(args);

// Add Razor Pages and Server Side Blazor
builder.Services.AddRazorPages();
builder.Services.AddServerSideBlazor();

// Register Syncfusion Blazor Service
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

**For Blazor WebAssembly (Program.cs):**

```csharp
using Microsoft.AspNetCore.Components.Web;
using Microsoft.AspNetCore.Components.WebAssembly.Hosting;
using Syncfusion.Blazor;

var builder = WebAssemblyHostBuilder.CreateDefault(args);
builder.RootComponents.Add<App>("#app");
builder.RootComponents.Add<HeadOutlet>("head::after");

builder.Services.AddScoped(sp => new HttpClient { BaseAddress = new Uri(builder.HostEnvironment.BaseAddress) });

// Register Syncfusion Blazor Service
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

// Register Syncfusion Blazor Service
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

### Step 4: Add Theme Stylesheet

Add the Syncfusion theme stylesheet to your application.

**For Blazor Server and Web App:**

Add to `Pages/_Host.cshtml` or `Pages/_Layout.cshtml` (inside `<head>` tag):

```html
<head>
    <!-- ... other head content ... -->
    <link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
</head>
```

**For Blazor WebAssembly:**

Add to `wwwroot/index.html` (inside `<head>` tag):

```html
<head>
    <!-- ... other head content ... -->
    <link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
</head>
```

**Available Themes:**
- `bootstrap5.css` - Bootstrap 5 theme
- `material.css` - Material theme
- `fabric.css` - Fabric theme
- `fluent.css` - Fluent theme
- `tailwind.css` - Tailwind CSS theme
- `bootstrap5-dark.css` - Bootstrap 5 dark theme
- `material-dark.css` - Material dark theme
- `fluent-dark.css` - Fluent dark theme
- `tailwind-dark.css` - Tailwind dark theme

## Your First Bullet Chart

Create a simple bullet chart displaying a revenue KPI:

**BasicBulletChart.razor:**

```razor
@page "/basic-bullet-chart"
@using Syncfusion.Blazor.Charts

<h3>Revenue Performance</h3>

<SfBulletChart DataSource="@RevenueData" 
               ValueField="ActualRevenue" 
               TargetField="TargetRevenue" 
               Minimum="0" 
               Maximum="300" 
               Interval="50" 
               Title="Revenue (in thousands $)">
    <BulletChartRangeCollection>
        <BulletChartRange End="150" Color="#d32f2f"></BulletChartRange>
        <BulletChartRange End="250" Color="#ffa726"></BulletChartRange>
        <BulletChartRange End="300" Color="#66bb6a"></BulletChartRange>
    </BulletChartRangeCollection>
</SfBulletChart>

@code {
    public class RevenueMetric
    {
        public double ActualRevenue { get; set; }
        public double TargetRevenue { get; set; }
    }
    
    public List<RevenueMetric> RevenueData = new List<RevenueMetric>
    {
        new RevenueMetric { ActualRevenue = 270, TargetRevenue = 250 }
    };
}
```

**What this creates:**
- A horizontal bullet chart with title
- Actual revenue bar at $270K
- Target marker at $250K
- Three qualitative ranges: Poor (0-150), Satisfactory (150-250), Good (250-300)
- Bootstrap 5 theme styling

## Project Structure

Your project should now include:

```
YourBlazorApp/
├── Pages/
│   └── BasicBulletChart.razor
├── _Imports.razor (with Syncfusion namespaces)
├── Program.cs (with AddSyncfusionBlazor())
└── wwwroot/ or Pages/
    └── _Host.cshtml or _Layout.cshtml or index.html (with theme CSS)
```

## Troubleshooting

### Issue: "The type or namespace name 'Syncfusion' could not be found"

**Solution:**
1. Verify NuGet packages are installed: `Syncfusion.Blazor.BulletChart` and `Syncfusion.Blazor.Themes`
2. Check `_Imports.razor` contains `@using Syncfusion.Blazor.Charts`
3. Restore NuGet packages: `dotnet restore`

### Issue: Chart displays but has no styling

**Solution:**
1. Verify theme stylesheet is referenced in `_Host.cshtml`, `_Layout.cshtml`, or `index.html`
2. Check the path: `_content/Syncfusion.Blazor.Themes/bootstrap5.css`
3. Clear browser cache and rebuild the project

### Issue: "AddSyncfusionBlazor is not found"

**Solution:**
1. Add `using Syncfusion.Blazor;` at the top of `Program.cs`
2. Ensure `Syncfusion.Blazor.BulletChart` package is installed (it includes the required dependencies)

### Issue: Chart not displaying in Blazor WebAssembly

**Solution:**
1. Ensure `AddSyncfusionBlazor()` is called in the WebAssembly `Program.cs`
2. Verify theme CSS is in `wwwroot/index.html`
3. Check browser console for JavaScript errors
4. Ensure static files are being served correctly

