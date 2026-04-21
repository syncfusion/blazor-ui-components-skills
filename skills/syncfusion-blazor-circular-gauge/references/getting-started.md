# Getting Started with Blazor CircularGauge

This guide explains how to set up and create your first Syncfusion Blazor CircularGauge component in a Blazor WebAssembly application.

## Table of Contents
- [Prerequisites](#prerequisites)
- [Installation Steps](#installation-steps)
    - [Option 1: Visual Studio](#option-1-visual-studio)
    - [Option 2: Visual Studio Code](#option-2-visual-studio-code)
    - [Option 3: .NET CLI](#option-3-net-cli)
- [Configuration Steps](#configuration-steps)
    - [Step 1: Add Import Namespaces](#step-1-add-import-namespaces)
    - [Step 2: Register Syncfusion Blazor Service](#step-2-register-syncfusion-blazor-service)
    - [Step 3: Add Stylesheet and Script Resources](#step-3-add-stylesheet-and-script-resources)
- [Creating Your First CircularGauge](#creating-your-first-circulargauge)
    - [Basic Gauge](#basic-gauge)
    - [Set Pointer Value](#set-pointer-value)
    - [Adding a Title](#adding-a-title)
    - [Adding Ranges](#adding-ranges)
- [Complete Example](#complete-example)
- [Project Structure](#project-structure)
- [Running the Application](#running-the-application)
- [Common Issues and Troubleshooting](#common-issues-and-troubleshooting)
    - [Issue: Gauge Not Rendering](#issue-gauge-not-rendering)
    - [Issue: Styles Not Applied](#issue-styles-not-applied)
    - [Issue: Script Errors](#issue-script-errors)
    - [Issue: Component Namespace Not Found](#issue-component-namespace-not-found)
- [Next Steps](#next-steps)
- [Additional Resources](#additional-resources)
- [Key Takeaways](#key-takeaways)

## Prerequisites

Before you begin, ensure you have:
- **System Requirements**: Compatible with the [Blazor component system requirements](https://blazor.syncfusion.com/documentation/system-requirements)
- **Development Environment**: Visual Studio, Visual Studio Code, or .NET CLI
- **Blazor Knowledge**: Basic understanding of Blazor applications

## Installation Steps

### Option 1: Visual Studio

#### Step 1: Create a Blazor WebAssembly App

1. Open Visual Studio
2. Create a **Blazor WebAssembly App** using Microsoft Templates or Syncfusion Blazor Extension
3. For detailed instructions, refer to [Blazor WASM App Getting Started](https://blazor.syncfusion.com/documentation/getting-started/blazor-webassembly-app)

#### Step 2: Install NuGet Packages

**Via NuGet Package Manager UI:**
1. Navigate to **Tools → NuGet Package Manager → Manage NuGet Packages for Solution**
2. Search and install:
   - `Syncfusion.Blazor.CircularGauge`
   - `Syncfusion.Blazor.Themes`

**Via Package Manager Console:**

```powershell
Install-Package Syncfusion.Blazor.CircularGauge -Version 27.1.48
Install-Package Syncfusion.Blazor.Themes -Version 27.1.48
```

> **Note**: Replace version numbers with the latest available version. Syncfusion Blazor components are available at [nuget.org](https://www.nuget.org/packages?q=syncfusion.blazor).

### Option 2: Visual Studio Code

#### Step 1: Create a Blazor WebAssembly App

1. Open Visual Studio Code
2. Create via Syncfusion Blazor Extension or use the integrated terminal:

```bash
dotnet new blazorwasm -o BlazorApp
cd BlazorApp
```

#### Step 2: Install NuGet Packages

Open the integrated terminal (Ctrl + `) in the project root directory and run:

```bash
dotnet add package Syncfusion.Blazor.CircularGauge -v 27.1.48
dotnet add package Syncfusion.Blazor.Themes -v 27.1.48
dotnet restore
```

### Option 3: .NET CLI

#### Step 1: Verify .NET SDK Installation

Check your .NET SDK version:

```bash
dotnet --version
```

#### Step 2: Create a Blazor WebAssembly App

```bash
dotnet new blazorwasm -o BlazorApp
cd BlazorApp
```

#### Step 3: Install NuGet Packages

```bash
dotnet add package Syncfusion.Blazor.CircularGauge -Version 27.1.48
dotnet add package Syncfusion.Blazor.Themes -Version 27.1.48
dotnet restore
```

## Configuration Steps

### Step 1: Add Import Namespaces

Open **~/_Imports.razor** and add the following namespaces:

```csharp
@using Syncfusion.Blazor
@using Syncfusion.Blazor.CircularGauge
```

This makes the CircularGauge component available throughout your application.

### Step 2: Register Syncfusion Blazor Service

Open **~/Program.cs** and register the Syncfusion Blazor service:

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

### Step 3: Add Stylesheet and Script Resources

Add the theme stylesheet and script references in the `<head>` section of **~/index.html**:

```html
<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>BlazorApp</title>
    <base href="/" />
    
    <!-- Syncfusion Blazor Theme -->
    <link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
    
    <!-- Syncfusion Blazor Script -->
    <script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js" type="text/javascript"></script>
</head>
```

**Available Themes:**
- `bootstrap5.css` (default)
- `material.css`
- `fabric.css`
- `fluent.css`
- `bootstrap.css`
- `bootstrap4.css`
- `tailwind.css`
- `tailwind-dark.css`
- `material-dark.css`
- `bootstrap-dark.css`
- `bootstrap5-dark.css`
- `fluent-dark.css`
- `fabric-dark.css`

> **Note**: You can also reference themes via CDN or use the Custom Resource Generator (CRG). See [Blazor Themes documentation](https://blazor.syncfusion.com/documentation/appearance/themes) for more options.

## Creating Your First CircularGauge

### Basic Gauge

Add the CircularGauge component in **~/Pages/Index.razor**:

```cshtml
@page "/"
@using Syncfusion.Blazor.CircularGauge

<h3>My First Circular Gauge</h3>

<SfCircularGauge>
    <CircularGaugeAxes>
        <CircularGaugeAxis>
            <CircularGaugePointers>
                <CircularGaugePointer></CircularGaugePointer>
            </CircularGaugePointers>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

**Run the Application:**
- **Windows**: Press `Ctrl + F5`
- **macOS**: Press `⌘ + F5`

This renders a basic circular gauge with default settings (scale 0-100, pointer at 0).

### Set Pointer Value

Modify the pointer value using the `Value` property:

```cshtml
@using Syncfusion.Blazor.CircularGauge
<SfCircularGauge>
    <CircularGaugeAxes>
        <CircularGaugeAxis>
            <CircularGaugePointers>
                <CircularGaugePointer Value="35">
                </CircularGaugePointer>
            </CircularGaugePointers>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

The pointer now points to 35 on the scale.

> **Note**: You can configure multiple axes in a CircularGauge, and each axis can have multiple pointers.

### Adding a Title

Provide context to your gauge with a title:

```cshtml
@using Syncfusion.Blazor.CircularGauge
<SfCircularGauge Title="Speedometer">
    <CircularGaugeTitleStyle Color="blue" FontWeight="bold" Size="25">
    </CircularGaugeTitleStyle>
    <CircularGaugeAxes>
        <CircularGaugeAxis>
            <CircularGaugePointers>
                <CircularGaugePointer Value="35">
                </CircularGaugePointer>
            </CircularGaugePointers>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

**Title Properties:**
- `Color`: Title text color
- `FontWeight`: Font weight (normal, bold, etc.)
- `Size`: Font size (e.g., "25", "18px")
- `FontFamily`: Font family
- `FontStyle`: Font style (normal, italic)

### Adding Ranges

Ranges help visualize different zones or thresholds on the gauge:

```cshtml
@using Syncfusion.Blazor.CircularGauge
<SfCircularGauge>
    <CircularGaugeAxes>
        <CircularGaugeAxis Minimum="0" Maximum="100">
            <CircularGaugeRanges>
                <CircularGaugeRange Start="0" End="40" Color="#30B32D">
                </CircularGaugeRange>
                <CircularGaugeRange Start="40" End="80" Color="#FFDD00">
                </CircularGaugeRange>
                <CircularGaugeRange Start="80" End="100" Color="#F03E3E">
                </CircularGaugeRange>
            </CircularGaugeRanges>
            <CircularGaugePointers>
                <CircularGaugePointer Value="65">
                </CircularGaugePointer>
            </CircularGaugePointers>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

This creates three color-coded zones:
- **Green** (0-40): Safe zone
- **Yellow** (40-80): Warning zone
- **Red** (80-100): Critical zone

## Complete Example

Here's a complete example combining all the elements:

```cshtml
@page "/gauge-demo"
@using Syncfusion.Blazor.CircularGauge

<h3>Speed Monitor</h3>

<SfCircularGauge Title="Vehicle Speed" Width="400px" Height="400px">
    <CircularGaugeTitleStyle Color="#333" FontWeight="bold" Size="20">
    </CircularGaugeTitleStyle>
    <CircularGaugeAxes>
        <CircularGaugeAxis Minimum="0" Maximum="120" StartAngle="220" EndAngle="140">
            <CircularGaugeAxisLabelStyle>
                <CircularGaugeAxisLabelFont Size="12px"></CircularGaugeAxisLabelFont>
            </CircularGaugeAxisLabelStyle>
            <CircularGaugeAxisLineStyle Width="10" Color="#E0E0E0">
            </CircularGaugeAxisLineStyle>
            <CircularGaugeRanges>
                <CircularGaugeRange Start="0" End="40" Color="#30B32D">
                </CircularGaugeRange>
                <CircularGaugeRange Start="40" End="80" Color="#FFDD00">
                </CircularGaugeRange>
                <CircularGaugeRange Start="80" End="120" Color="#F03E3E">
                </CircularGaugeRange>
            </CircularGaugeRanges>
            <CircularGaugePointers>
                <CircularGaugePointer Value="65" Radius="60%" Color="#333">
                    <CircularGaugePointerAnimation Enable="true" Duration="1500">
                    </CircularGaugePointerAnimation>
                </CircularGaugePointer>
            </CircularGaugePointers>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

## Project Structure

After setup, your project structure should look like this:

```
BlazorApp/
├── Pages/
│   ├── Index.razor          (Your gauge implementation)
│   └── ...
├── Shared/
│   └── ...
├── wwwroot/
│   └── index.html           (Theme and script references)
├── _Imports.razor           (Namespace imports)
├── Program.cs               (Service registration)
└── BlazorApp.csproj         (NuGet package references)
```

## Running the Application

1. **Build the Project**: Ensure no compilation errors
2. **Run**: Press `Ctrl + F5` (Windows) or `⌘ + F5` (macOS)
3. **View**: The gauge renders in your default browser

## Common Issues and Troubleshooting

### Issue: Gauge Not Rendering

**Solution:**
1. Verify NuGet packages are installed
2. Check that `AddSyncfusionBlazor()` is called in Program.cs
3. Ensure theme CSS and script are referenced in index.html
4. Confirm namespaces are imported in _Imports.razor

### Issue: Styles Not Applied

**Solution:**
1. Check theme CSS path in index.html
2. Verify the CSS file is served correctly (check browser DevTools)
3. Try using CDN instead: `https://cdn.syncfusion.com/blazor/{version}/styles/bootstrap5.css`

### Issue: Script Errors

**Solution:**
1. Ensure `syncfusion-blazor.min.js` is referenced after the closing `</body>` tag or in `<head>`
2. Verify script path is correct
3. Check browser console for specific error messages

### Issue: Component Namespace Not Found

**Solution:**
1. Rebuild the project after installing NuGet packages
2. Restart Visual Studio/VS Code
3. Run `dotnet restore` in the terminal

## Next Steps

Now that you have a basic CircularGauge running, explore these features:

1. **Axes Configuration**: Customize axis appearance, labels, ticks, and ranges
2. **Pointer Types**: Use needle, range bar, or marker pointers
3. **Annotations**: Add text, images, or custom HTML content
4. **User Interaction**: Enable tooltips and pointer dragging
5. **Legends**: Display legends for ranges
6. **Animations**: Configure smooth pointer animations
7. **Themes**: Apply different built-in themes or create custom styles
8. **Events**: Handle gauge events for interactivity
9. **Real-Time Updates**: Update pointer values dynamically
10. **Accessibility**: Ensure WCAG compliance for all users

## Additional Resources

- **GitHub Sample**: [View complete getting started sample](https://github.com/SyncfusionExamples/Blazor-Getting-Started-Examples/tree/main/CircularGauge)
- **Live Demos**: Explore [Blazor CircularGauge demos](https://blazor.syncfusion.com/demos/circular-gauge/default-functionalities)
- **API Documentation**: [CircularGauge API reference](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.CircularGauge.html)
- **Theme Documentation**: [Blazor Themes guide](https://blazor.syncfusion.com/documentation/appearance/themes)
- **Server App Setup**: For Blazor Server apps, see [getting started with server app](https://blazor.syncfusion.com/documentation/getting-started/blazor-server-app)
- **Web App Setup**: For Blazor Web App (.NET 8+), see [getting started with web app](https://blazor.syncfusion.com/documentation/getting-started/blazor-web-app)

## Key Takeaways

- **Three NuGet packages required**: CircularGauge, Themes, and Core (dependency)
- **Service registration is mandatory**: Call `AddSyncfusionBlazor()` in Program.cs
- **Theme CSS and script must be referenced**: Add to index.html
- **Basic structure**: `SfCircularGauge` → `CircularGaugeAxes` → `CircularGaugeAxis` → `CircularGaugePointers`
- **Ranges are optional but powerful**: Use them to create visual zones
- **Start simple, then enhance**: Begin with a basic gauge, then add features incrementally
