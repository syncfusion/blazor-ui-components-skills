# Getting Started with Blazor Linear Gauge

This comprehensive guide covers everything needed to set up and implement the Syncfusion Blazor Linear Gauge component from scratch.

## Table of Contents
- [Overview](#overview)
- [Prerequisites](#prerequisites)
- [Installation and Setup](#installation-and-setup)
- [Configuration Steps](#configuration-steps)
- [Basic Implementation](#basic-implementation)
- [Adding a Title](#adding-a-title)
- [Adding Ranges](#adding-ranges)
- [Horizontal Orientation](#horizontal-orientation)
- [Complete Working Example](#complete-working-example)
- [Key Concepts Summary](#key-concepts-summary)
- [Next Steps](#next-steps)
- [Troubleshooting](#troubleshooting)
- [Sample Project Structure](#sample-project-structure)
- [Additional Resources](#additional-resources)


## Overview

The Blazor LinearGauge (`SfLinearGauge`) is an ideal component for visualizing numeric values in a linear scale with features like multiple axes, different orientations, ranges, pointers, and more. This guide walks through installation, configuration, and basic implementation.

## Prerequisites

Before starting, ensure you have:
- [System requirements for Blazor components](https://blazor.syncfusion.com/documentation/system-requirements) met
- Visual Studio, Visual Studio Code, or .NET CLI installed
- Basic understanding of Blazor WebAssembly or Blazor Server applications

## Installation and Setup

The Blazor Linear Gauge can be set up using three different development environments. Choose the one that matches your workflow.

### Option 1: Visual Studio

#### Create a New Blazor App

1. Open Visual Studio
2. Create a **Blazor WebAssembly App** or **Blazor Server App** using [Microsoft Templates](https://learn.microsoft.com/en-us/aspnet/core/blazor/tooling)
3. For detailed instructions, refer to [Blazor WASM App Getting Started](https://blazor.syncfusion.com/documentation/getting-started/blazor-webassembly-app)

#### Install NuGet Packages

1. Open **Tools → NuGet Package Manager → Manage NuGet Packages for Solution**
2. Search for and install:
   - `Syncfusion.Blazor.LinearGauge`
   - `Syncfusion.Blazor.Themes`

**Package Manager Console alternative:**
```powershell
Install-Package Syncfusion.Blazor.LinearGauge -Version 27.1.48
Install-Package Syncfusion.Blazor.Themes -Version 27.1.48
```

> **Note:** Replace version number with the latest available version from [nuget.org](https://www.nuget.org/packages/Syncfusion.Blazor.LinearGauge).

### Option 2: Visual Studio Code

#### Create a New Blazor App

1. Open Visual Studio Code
2. Open integrated terminal (<kbd>Ctrl</kbd>+<kbd>`</kbd>)
3. Run the following commands:

```bash
dotnet new blazorwasm -o BlazorLinearGaugeApp
cd BlazorLinearGaugeApp
```

#### Install NuGet Packages

In the integrated terminal, run:

```bash
dotnet add package Syncfusion.Blazor.LinearGauge
dotnet add package Syncfusion.Blazor.Themes
dotnet restore
```

### Option 3: .NET CLI

#### Verify .NET SDK Installation

Check your .NET SDK version:

```bash
dotnet --version
```

#### Create a New Blazor App

```bash
dotnet new blazorwasm -o BlazorLinearGaugeApp
cd BlazorLinearGaugeApp
```

#### Install NuGet Packages

```bash
dotnet add package Syncfusion.Blazor.LinearGauge
dotnet add package Syncfusion.Blazor.Themes
dotnet restore
```

## Configuration Steps

After installing the packages, configure the application to use the Linear Gauge component.

### Step 1: Import Namespaces

Open **~/_Imports.razor** and add the following namespaces:

```razor
@using Syncfusion.Blazor
@using Syncfusion.Blazor.LinearGauge
```

### Step 2: Register Syncfusion Blazor Service

Open **~/Program.cs** and register the Syncfusion Blazor service:

**For Blazor WebAssembly:**

```csharp
using Microsoft.AspNetCore.Components.Web;
using Microsoft.AspNetCore.Components.WebAssembly.Hosting;
using Syncfusion.Blazor;

var builder = WebAssemblyHostBuilder.CreateDefault(args);
builder.RootComponents.Add<App>("#app");
builder.RootComponents.Add<HeadOutlet>("head::after");

builder.Services.AddScoped(sp => new HttpClient 
{ 
    BaseAddress = new Uri(builder.HostEnvironment.BaseAddress) 
});

// Register Syncfusion Blazor Service
builder.Services.AddSyncfusionBlazor();

await builder.Build().RunAsync();
```

**For Blazor Server:**

```csharp
using Syncfusion.Blazor;

var builder = WebApplication.CreateBuilder(args);

// Add services to the container.
builder.Services.AddRazorPages();
builder.Services.AddServerSideBlazor();

// Register Syncfusion Blazor Service
builder.Services.AddSyncfusionBlazor();

var app = builder.Build();
// ... rest of the configuration
```

### Step 3: Add Stylesheet and Script References

Add the theme stylesheet and script reference in the `<head>` section of **~/index.html** (Blazor WebAssembly) or **~/_Layout.cshtml** / **~/Pages/_Host.cshtml** (Blazor Server):

```html
<head>
    <!-- Other head content -->
    <link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
    <script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js" type="text/javascript"></script>
</head>
```

**Available themes:**
- `bootstrap5.css` - Bootstrap 5 theme
- `material.css` - Material theme
- `fabric.css` - Fabric theme
- `bootstrap.css` - Bootstrap 4 theme
- `tailwind.css` - Tailwind CSS theme
- `fluent.css` - Fluent theme
- `material3.css` - Material 3 theme

> **Alternative approaches:** Check out [Blazor Themes documentation](https://blazor.syncfusion.com/documentation/appearance/themes) for CDN and Custom Resource Generator (CRG) options.

## Basic Implementation

Now that the setup is complete, let's create your first Linear Gauge.

### Minimal Linear Gauge

Add the following code to **~/Pages/Index.razor**:

```razor
@page "/"
@using Syncfusion.Blazor.LinearGauge

<h3>Linear Gauge</h3>

<SfLinearGauge>
    <LinearGaugeAxes>
        <LinearGaugeAxis>
            <LinearGaugePointers>
                <LinearGaugePointer></LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

**Run the application:**
- **Visual Studio:** Press <kbd>Ctrl</kbd>+<kbd>F5</kbd> (Windows) or <kbd>⌘</kbd>+<kbd>F5</kbd> (macOS)
- **VS Code/.NET CLI:** Run `dotnet run` in terminal

**Result:** A vertical linear gauge with default axis (0 to 100) and a pointer at the default position.

### Setting Pointer Value

To display a specific value, set the `PointerValue` property:

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeAxes>
        <LinearGaugeAxis>
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="40"></LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

**Result:** The pointer now indicates value 40 on the scale.

### Adding Multiple Pointers

You can add multiple pointers to show different values on the same axis:

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeAxes>
        <LinearGaugeAxis>
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="40" Color="#007bff"></LinearGaugePointer>
                <LinearGaugePointer PointerValue="70" Color="#dc3545"></LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

**Result:** Two pointers at values 40 and 70 with different colors.

## Adding a Title

Provide context to your gauge with a descriptive title:

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge Title="Temperature Monitor">
    <LinearGaugeAxes>
        <LinearGaugeAxis>
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="45"></LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

**Result:** The gauge displays "Temperature Monitor" as the title at the top.

## Adding Ranges

Ranges visually categorize the scale into colored segments. They're useful for showing different zones (e.g., safe, warning, critical).

### Single Range Example

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeAxes>
        <LinearGaugeAxis Minimum="0" Maximum="100">
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="60"></LinearGaugePointer>
            </LinearGaugePointers>
            <LinearGaugeRanges>
                <LinearGaugeRange Start="50" End="80" Color="#FFA500"></LinearGaugeRange>
            </LinearGaugeRanges>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

### Multiple Ranges Example

Create color-coded zones for better visualization:

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge Title="Temperature Gauge">
    <LinearGaugeAxes>
        <LinearGaugeAxis Minimum="0" Maximum="200">
            <LinearGaugeAxisLabelStyle Format="{value}°C"></LinearGaugeAxisLabelStyle>
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="140"></LinearGaugePointer>
            </LinearGaugePointers>
            <LinearGaugeRanges>
                <LinearGaugeRange Start="0" End="80" Color="#ff5985"></LinearGaugeRange>
                <LinearGaugeRange Start="80" End="120" Color="#ffb133"></LinearGaugeRange>
                <LinearGaugeRange Start="120" End="140" Color="#fcde0b"></LinearGaugeRange>
                <LinearGaugeRange Start="140" End="200" Color="#27d5ff"></LinearGaugeRange>
            </LinearGaugeRanges>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

**Result:** A temperature gauge with four color-coded ranges showing different temperature zones, with labels formatted as degrees Celsius.

## Horizontal Orientation

Change the gauge orientation to horizontal for dashboard or panel layouts:

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge Orientation="Syncfusion.Blazor.LinearGauge.Orientation.Horizontal" Width="600px" Height="150px">
    <LinearGaugeAxes>
        <LinearGaugeAxis>
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="60"></LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

**Result:** A horizontal linear gauge instead of the default vertical orientation.

## Complete Working Example

Here's a complete example combining title, ranges, pointer, and formatting:

```razor
@page "/lineargauge"
@using Syncfusion.Blazor.LinearGauge

<div class="gauge-container">
    <h3>Server Performance Monitor</h3>
    
    <SfLinearGauge Title="CPU Usage %" Width="200px" Height="400px">
        <LinearGaugeTitleStyle Size="16px"></LinearGaugeTitleStyle>
        <LinearGaugeAxes>
            <LinearGaugeAxis Minimum="0" Maximum="100">
                <LinearGaugeAxisLabelStyle Format="{value}%">
                </LinearGaugeAxisLabelStyle>
                
                <LinearGaugePointers>
                    <LinearGaugePointer PointerValue="@cpuUsage" 
                                        Color="#007bff"
                                        AnimationDuration="1000">
                    </LinearGaugePointer>
                </LinearGaugePointers>
                
                <LinearGaugeRanges>
                    <LinearGaugeRange Start="0" End="50" Color="#4CAF50" 
                                      StartWidth="10" EndWidth="10">
                    </LinearGaugeRange>
                    <LinearGaugeRange Start="50" End="75" Color="#FFC107" 
                                      StartWidth="10" EndWidth="10">
                    </LinearGaugeRange>
                    <LinearGaugeRange Start="75" End="100" Color="#F44336" 
                                      StartWidth="10" EndWidth="10">
                    </LinearGaugeRange>
                </LinearGaugeRanges>
            </LinearGaugeAxis>
        </LinearGaugeAxes>
    </SfLinearGauge>
</div>

@code {
    private double cpuUsage = 65;
    
    protected override void OnInitialized()
    {
        // In a real application, fetch actual CPU usage
        // cpuUsage = GetCurrentCPUUsage();
    }
}

<style>
    .gauge-container {
        padding: 20px;
        text-align: center;
    }
</style>
```

## Key Concepts Summary

### Essential Components

1. **SfLinearGauge** - The main container component
2. **LinearGaugeAxes** - Collection of axes (supports multiple axes)
3. **LinearGaugeAxis** - Individual axis defining the scale
4. **LinearGaugePointers** - Collection of pointers on an axis
5. **LinearGaugePointer** - Individual pointer showing a value
6. **LinearGaugeRanges** - Collection of colored ranges
7. **LinearGaugeRange** - Individual range segment

### Default Values

- **Axis Minimum:** 0
- **Axis Maximum:** 100
- **Pointer Value:** 0
- **Orientation:** Vertical
- **Pointer Type:** Marker (InvertedTriangle)
- **Width:** Auto
- **Height:** Auto (450px when vertical, 150px when horizontal)

## Next Steps

Now that you have a working Linear Gauge:

1. **Customize Axes:** Modify tick marks, labels, and line appearance
2. **Explore Pointer Types:** Try bar pointers, different marker shapes, image pointers
3. **Add Interactivity:** Enable pointer dragging, tooltips
4. **Style the Gauge:** Apply themes, customize colors, fonts
5. **Add Annotations:** Include custom text or HTML content
6. **Implement Events:** Respond to user actions and value changes

## Troubleshooting

### Common Issues

**Issue:** Gauge not displaying
- **Solution:** Verify stylesheet and script references in index.html
- **Solution:** Ensure `AddSyncfusionBlazor()` is called in Program.cs
- **Solution:** Check that namespaces are imported in _Imports.razor

**Issue:** Pointer not visible
- **Solution:** Ensure `PointerValue` is within the axis Minimum and Maximum range
- **Solution:** Check if pointer color matches background (try changing color)

**Issue:** Compilation errors
- **Solution:** Verify NuGet package versions are compatible
- **Solution:** Run `dotnet restore` to ensure packages are properly installed

**Issue:** Theme not applied
- **Solution:** Check the stylesheet path matches your chosen theme
- **Solution:** Verify the file exists in the application's static web assets

## Sample Project Structure

After setup, your project should have:

```
BlazorLinearGaugeApp/
├── Pages/
│   └── Index.razor (Your Linear Gauge code)
├── _Imports.razor (Namespace imports)
├── Program.cs (Service registration)
├── wwwroot/
│   └── index.html (Stylesheet and script references)
└── BlazorLinearGaugeApp.csproj (NuGet package references)
```

## Additional Resources

- **Sample Project:** [Blazor Linear Gauge Examples on GitHub](https://github.com/SyncfusionExamples/Blazor-Getting-Started-Examples/tree/main/LinearGauge)
- **API Reference:** [SfLinearGauge API Documentation](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.LinearGauge.SfLinearGauge.html)
- **Live Demos:** [Blazor Linear Gauge Demos](https://blazor.syncfusion.com/demos/linear-gauge/default-functionalities)

## Summary

You've successfully:
- ✅ Installed Syncfusion.Blazor.LinearGauge package
- ✅ Configured your Blazor application
- ✅ Created your first Linear Gauge
- ✅ Added pointers and ranges
- ✅ Customized the gauge with title and formatting

This foundation prepares you for more advanced features like custom animations, events, methods, and complex multi-axis gauges.
