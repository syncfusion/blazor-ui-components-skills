# Getting Started with TreeMap

## Table of Contents
- [Overview](#overview)
- [Prerequisites](#prerequisites)
- [Install Package](#install-package)
- [Minimal Example](#minimal-example)
- [Project Types](#project-types)
- [Common Issues](#common-issues)

## Overview
This reference explains how to add and render the Syncfusion Blazor `TreeMap` component in a Blazor application. It covers installation, project setup, and a minimal working example.

## Prerequisites
- .NET SDK installed
- Blazor project (WASM or Server)
- Syncfusion Blazor packages available in NuGet

## Install Package
Install the `Syncfusion.Blazor.TreeMap` NuGet package into your project and restore packages.

## Minimal Example
```csharp
@using Syncfusion.Blazor.TreeMap

<SfTreeMap WeightValuePath="EmployeeCount" TValue="Employee" DataSource="Employees" >
    <TreeMapLevels>
        <TreeMapLevel GroupPath="Country">
        </TreeMapLevel>
    </TreeMapLevels>
</SfTreeMap>

@code{
    public class Employee
    {
        public string Country { get; set; }
        public string JobDescription { get; set; }
        public string JobGroup { get; set; }
        public int EmployeeCount { get; set; }
    };
    public List<Employee> Employees = new List<Employee> {
        new Employee { Country= "USA", JobDescription= "Sales", JobGroup= "Executive", EmployeeCount= 20 },
        new Employee { Country= "USA", JobDescription= "Sales", JobGroup= "Analyst", EmployeeCount= 30 },
        new Employee { Country= "USA", JobDescription= "Marketing", EmployeeCount= 40 },
        new Employee { Country= "USA", JobDescription= "Management", EmployeeCount= 80 },
    };
}
```

## Project Types
- Visual Studio, Visual Studio Code, and .NET CLI instructions are supported. Use the standard Blazor project templates.

## Common Issues
- Missing NuGet package: run `dotnet add package Syncfusion.Blazor.TreeMap -v <version>`
- Rendering errors: ensure correct `DataSource` and `GroupPath` settings.
# Getting Started with Blazor TreeMap

A comprehensive guide for installing, configuring, and implementing the Syncfusion Blazor TreeMap component in your Blazor applications (WebAssembly, Server, and Web App).

## Table of Contents

- [Overview](#overview)
- [Prerequisites](#prerequisites)
- [Installation Methods](#installation-methods)
  - [Blazor WebAssembly Application](#blazor-webassembly-application)
  - [Blazor Web App](#blazor-web-app)
- [Project Configuration](#project-configuration)
  - [Import Namespaces](#import-namespaces)
  - [Register Syncfusion Services](#register-syncfusion-services)
  - [Add Script Resources](#add-script-resources)
  - [Add Theme Stylesheets](#add-theme-stylesheets)
- [Your First TreeMap Component](#your-first-treemap-component)
- [Adding Labels](#adding-labels)
- [Adding Title](#adding-title)
- [Applying Color Mapping](#applying-color-mapping)
- [Enabling Legend](#enabling-legend)
- [Enabling Tooltip](#enabling-tooltip)
- [License Registration](#license-registration)
- [Troubleshooting Common Issues](#troubleshooting-common-issues)

## Overview

The Syncfusion Blazor TreeMap component visualizes hierarchical data as nested rectangles, where rectangle size represents quantitative values and colors indicate additional dimensions. This guide covers installation and basic setup for all Blazor hosting models.

**What You'll Learn:**
- Installing the NuGet package across different development environments
- Configuring services and resources for Blazor WebAssembly, Server, and Web App
- Creating your first TreeMap with data binding
- Adding essential features (labels, title, color mapping, legend, tooltip)

**Estimated Time:** 15-20 minutes

## Prerequisites

Before starting, ensure you have:

- **Visual Studio 2022** (17.0 or later) **OR** **Visual Studio Code** with C# extension
- **.NET SDK 6.0 or later** - Check version with `dotnet --version`
- **Basic Blazor knowledge** - Understanding of components and data binding
- **Syncfusion License** - Free Community License or commercial license (required for production)

**System Requirements:**
- For detailed system requirements, visit: [Blazor System Requirements](https://blazor.syncfusion.com/documentation/system-requirements)

## Installation Methods

Choose the installation method based on your Blazor hosting model and development environment.

### Blazor WebAssembly Application

#### Using Visual Studio

**Step 1: Create Blazor WebAssembly Project**

1. Open Visual Studio 2022
2. Select **File → New → Project**
3. Search for "Blazor WebAssembly App" template
4. Configure project name and location
5. Click **Create**

**Step 2: Install NuGet Package**

**Option A: NuGet Package Manager UI**
1. Right-click project in Solution Explorer
2. Select **Manage NuGet Packages**
3. Click **Browse** tab
4. Search for `Syncfusion.Blazor.TreeMap`
5. Select latest version and click **Install**

**Option B: Package Manager Console**

```powershell
Install-Package Syncfusion.Blazor.TreeMap -Version 27.1.48
```

Replace `27.1.48` with the latest version available.

#### Using Visual Studio Code

**Step 1: Create Blazor WebAssembly Project**

Open integrated terminal (Ctrl+\`) and run:

```bash
dotnet new blazorwasm -o MyTreeMapApp
cd MyTreeMapApp
```

**Step 2: Install NuGet Package**

```bash
dotnet add package Syncfusion.Blazor.TreeMap
dotnet restore
```

#### Using .NET CLI (Command Line)

**Step 1: Verify .NET SDK Installation**

```bash
dotnet --version
```

Expected output: `8.0.x` or higher

**Step 2: Create Project**

```bash
dotnet new blazorwasm -o MyTreeMapApp
cd MyTreeMapApp
```

**Step 3: Install Package**

```bash
dotnet add package Syncfusion.Blazor.TreeMap
dotnet restore
```

### Blazor Web App

Blazor Web App (introduced in .NET 8) supports multiple render modes. Installation varies based on interactivity settings.

#### Using Visual Studio

**Step 1: Create Blazor Web App**

1. Open Visual Studio 2022
2. Select **File → New → Project**
3. Search for "Blazor Web App" template
4. Configure project settings
5. **Important:** Select **Interactive render mode** (Auto, WebAssembly, or Server)
6. Select **Interactivity location** (Global or Per page/component)
7. Click **Create**

![Blazor Web App configuration](https://ej2.syncfusion.com/blazor/documentation/images/blazor-create-web-app.png)

**Step 2: Install NuGet Package**

- **For Server render mode:** Install in the main server project only
- **For WebAssembly or Auto mode:** Install in **BOTH** server project AND `.Client` project

Use NuGet Package Manager or Package Manager Console:

```powershell
Install-Package Syncfusion.Blazor.TreeMap -Version 27.1.48
```

#### Using Visual Studio Code

**Step 1: Create Blazor Web App with Auto Render Mode**

```bash
dotnet new blazor -o MyTreeMapApp -int Auto
cd MyTreeMapApp
```

Available render mode options:
- `-int Server` - Server-side rendering only
- `-int WebAssembly` - Client-side WebAssembly only
- `-int Auto` - Automatic switching between Server and WebAssembly

**Step 2: Install NuGet Package**

For **WebAssembly** or **Auto** mode, navigate to client project:

```bash
cd MyTreeMapApp.Client
dotnet add package Syncfusion.Blazor.TreeMap
dotnet restore
cd ..
```

For **Server** mode, install in main project:

```bash
dotnet add package Syncfusion.Blazor.TreeMap
dotnet restore
```

#### Using .NET CLI

**Step 1: Create Project**

```bash
dotnet new blazor -o MyTreeMapApp -int Auto
cd MyTreeMapApp
```

**Step 2: Install Package**

For WebAssembly/Auto mode:

```bash
cd MyTreeMapApp.Client
dotnet add package Syncfusion.Blazor.TreeMap
dotnet restore
cd ..
```

## Project Configuration

After installing the NuGet package, configure your project to use Syncfusion components.

### Import Namespaces

Open `_Imports.razor` file (root of project for WebAssembly, or in `.Client` project for Web App) and add:

```razor
@using Syncfusion.Blazor
@using Syncfusion.Blazor.TreeMap
```

**File Location:**
- **WebAssembly:** `~/Imports.razor`
- **Web App (Auto/WebAssembly):** `~/.Client/_Imports.razor`
- **Web App (Server):** `~/_Imports.razor`

### Register Syncfusion Services

#### Blazor WebAssembly

Open `Program.cs` and register services:

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

#### Blazor Web App

**For Server or WebAssembly/Auto mode, register in BOTH files:**

**File 1: Main Project `Program.cs` (Server)**

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
// ... rest of configuration
```

**File 2: Client Project `Program.cs` (for WebAssembly/Auto mode)**

```csharp
using Microsoft.AspNetCore.Components.WebAssembly.Hosting;
using Syncfusion.Blazor;

var builder = WebAssemblyHostBuilder.CreateDefault(args);

// Register Syncfusion Blazor Service
builder.Services.AddSyncfusionBlazor();

await builder.Build().RunAsync();
```

### Add Script Resources

#### Blazor WebAssembly

Add script reference in `wwwroot/index.html` inside `<head>` section:

```html
<head>
    <meta charset="utf-8" />
    <title>My TreeMap App</title>
    <base href="/" />
    <link href="css/app.css" rel="stylesheet" />
    
    <!-- Syncfusion Blazor Script -->
    <script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js" 
            type="text/javascript"></script>
</head>
```

#### Blazor Web App

Add script reference at the end of `<body>` in `Components/App.razor`:

```html
<body>
    <Routes />
    
    <!-- Syncfusion Blazor Script -->
    <script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js" 
            type="text/javascript"></script>
</body>
```

**Alternative:** Individual script references for smaller bundle sizes - see [Adding Script Reference](https://blazor.syncfusion.com/documentation/common/adding-script-references).

### Add Theme Stylesheets

Add CSS theme reference in `<head>` section:

#### Blazor WebAssembly

`wwwroot/index.html`:

```html
<head>
    <!-- Syncfusion Blazor Theme -->
    <link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
</head>
```

#### Blazor Web App

`Components/App.razor`:

```html
<head>
    <HeadOutlet />
    
    <!-- Syncfusion Blazor Theme -->
    <link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
</head>
```

**Available Themes:**
- `bootstrap5.css` - Bootstrap 5 (recommended)
- `material3.css` - Material Design 3
- `fluent2.css` - Fluent Design 2
- `tailwind3.css` - Tailwind CSS 3
- `material.css` - Material Design (classic)
- `fabric.css` - Microsoft Fabric
- `bootstrap4.css` - Bootstrap 4

## Your First TreeMap Component

Now create your first TreeMap component with basic data binding.

### Blazor WebAssembly

Open `Pages/Index.razor` and replace content with:

```razor
@page "/"
@using Syncfusion.Blazor.TreeMap

<PageTitle>TreeMap Demo</PageTitle>

<h1>GDP by Country - 2015</h1>

<SfTreeMap DataSource="@GrowthReport"
            WeightValuePath="GDP"
            TValue="Country">
</SfTreeMap>

@code {
    public class Country
    {
        public string Name { get; set; }
        public double GDP { get; set; }
    }
    
    public List<Country> GrowthReport = new List<Country> 
    {
        new Country { Name = "United States", GDP = 17946 },
        new Country { Name = "China", GDP = 10866 },
        new Country { Name = "Japan", GDP = 4123 },
        new Country { Name = "Germany", GDP = 3355 },
        new Country { Name = "United Kingdom", GDP = 2848 },
        new Country { Name = "France", GDP = 2421 },
        new Country { Name = "India", GDP = 2073 },
        new Country { Name = "Italy", GDP = 1814 },
        new Country { Name = "Brazil", GDP = 1774 },
        new Country { Name = "Canada", GDP = 1550 }
    };
}
```

### Blazor Web App

For Web App, add render mode directive if interactivity location is "Per page/component":

**Create `Pages/TreeMapDemo.razor`:**

```razor
@page "/treemap-demo"
@rendermode InteractiveAuto
@using Syncfusion.Blazor.TreeMap

<PageTitle>TreeMap Demo</PageTitle>

<h1>GDP by Country - 2015</h1>

<SfTreeMap DataSource="@GrowthReport"
            WeightValuePath="GDP"
            TValue="Country">
</SfTreeMap>

@code {
    public class Country
    {
        public string Name { get; set; }
        public double GDP { get; set; }
    }
    
    public List<Country> GrowthReport = new List<Country> 
    {
        new Country { Name = "United States", GDP = 17946 },
        new Country { Name = "China", GDP = 10866 },
        new Country { Name = "Japan", GDP = 4123 },
        new Country { Name = "Germany", GDP = 3355 },
        new Country { Name = "United Kingdom", GDP = 2848 },
        new Country { Name = "France", GDP = 2421 },
        new Country { Name = "India", GDP = 2073 },
        new Country { Name = "Italy", GDP = 1814 },
        new Country { Name = "Brazil", GDP = 1774 },
        new Country { Name = "Canada", GDP = 1550 }
    };
}
```

**Render Mode Options:**
- `@rendermode InteractiveAuto` - Automatic (recommended)
- `@rendermode InteractiveWebAssembly` - Client-side only
- `@rendermode InteractiveServer` - Server-side only
- **Omit directive** if interactivity location is "Global"

### Run the Application

Press `Ctrl+F5` (Windows) or `⌘+F5` (macOS) to run without debugging.

**Expected Output:** TreeMap displaying 10 countries with rectangle sizes proportional to GDP values.

## Adding Labels

Display text labels on TreeMap items by specifying the `LabelPath` property:

```razor
@using Syncfusion.Blazor.TreeMap

<SfTreeMap DataSource="@GrowthReport"
            WeightValuePath="GDP"
            TValue="Country">
    <TreeMapLeafItemSettings LabelPath="Name" Fill="lightgray">
    </TreeMapLeafItemSettings>
</SfTreeMap>
```

**Key Properties:**
- `LabelPath="Name"` - Property from data source to display as label
- `Fill="lightgray"` - Background color for leaf items

**Result:** Each rectangle now shows the country name.

## Adding Title

Add a descriptive title using `TreeMapTitleSettings`:

```razor
@using Syncfusion.Blazor.TreeMap

<SfTreeMap DataSource="@GrowthReport"
            WeightValuePath="GDP"
            TValue="Country">
    <TreeMapTitleSettings Text="Top 10 Countries by GDP Nominal - 2015">
    </TreeMapTitleSettings>
    <TreeMapLeafItemSettings LabelPath="Name" Fill="lightgray">
    </TreeMapLeafItemSettings>
</SfTreeMap>
```

**Customization Options:**

```razor
@using Syncfusion.Blazor.TreeMap

<TreeMapTitleSettings Text="Top 10 Countries by GDP Nominal - 2015">
    <TreeMapTitleTextStyle Size="18px" 
                           FontWeight="Bold" 
                           Color="#333333" 
                           FontFamily="Arial">
    </TreeMapTitleTextStyle>
    <TreeMapSubtitleSettings Text="Source: World Bank 2015">
        <TreeMapSubtitleTextStyle Size="12px" Color="#666666">
        </TreeMapSubtitleTextStyle>
    </TreeMapSubtitleSettings>
</TreeMapTitleSettings>
```

## Applying Color Mapping

Use color mapping to represent additional data dimensions through colors.

### Range Color Mapping

Assign colors based on value ranges:

```razor
@using Syncfusion.Blazor.TreeMap

<SfTreeMap DataSource="@GrowthReport"
            WeightValuePath="GDP"
            RangeColorValuePath="GDP"
            TValue="Country">
    <TreeMapTitleSettings Text="Top 10 Countries by GDP Nominal - 2015">
    </TreeMapTitleSettings>
    <TreeMapLeafItemSettings LabelPath="Name">
        <TreeMapLeafColorMappings>
            <TreeMapLeafColorMapping StartRange="0" 
                                      EndRange="3000" 
                                      Color="@(new string[] { "Orange" })">
            </TreeMapLeafColorMapping>
            <TreeMapLeafColorMapping StartRange="3000" 
                                      EndRange="20000" 
                                      Color="@(new string[] { "Green" })">
            </TreeMapLeafColorMapping>
        </TreeMapLeafColorMappings>
    </TreeMapLeafItemSettings>
</SfTreeMap>
```

**Key Properties:**
- `RangeColorValuePath="GDP"` - Property used for color calculation
- `StartRange` / `EndRange` - Value range for color application
- `Color` - Array of colors (single color or gradient)

**Result:** Countries with GDP < 3000 appear orange, >= 3000 appear green.

## Enabling Legend

Add a legend to explain color mappings:

```razor
@using Syncfusion.Blazor.TreeMap

<SfTreeMap DataSource="@GrowthReport"
            WeightValuePath="GDP"
            RangeColorValuePath="GDP"
            TValue="Country">
    <TreeMapTitleSettings Text="Top 10 Countries by GDP Nominal - 2015">
    </TreeMapTitleSettings>
    <TreeMapLeafItemSettings LabelPath="Name">
        <TreeMapLeafColorMappings>
            <TreeMapLeafColorMapping StartRange="0" 
                                      EndRange="3000" 
                                      Color="@(new string[] { "Orange" })"
                                      Label="Low GDP">
            </TreeMapLeafColorMapping>
            <TreeMapLeafColorMapping StartRange="3000" 
                                      EndRange="20000" 
                                      Color="@(new string[] { "Green" })"
                                      Label="High GDP">
            </TreeMapLeafColorMapping>
        </TreeMapLeafColorMappings>
    </TreeMapLeafItemSettings>
    <TreeMapLegendSettings Visible="true" Position="Syncfusion.Blazor.TreeMap.LegendPosition.Top">
    </TreeMapLegendSettings>
</SfTreeMap>
```

**Legend Customization:**

```razor
@using Syncfusion.Blazor.TreeMap

<TreeMapLegendSettings Visible="true" 
                       Position="Syncfusion.Blazor.TreeMap.LegendPosition.Top"
                       Mode="Syncfusion.Blazor.TreeMap.LegendMode.Default"
                       Height="50px"
                       Width="200px">
    <TreeMapLegendTextStyle Size="14px" Color="#000000">
    </TreeMapLegendTextStyle>
</TreeMapLegendSettings>
```

**Position Options:** `Top`, `Bottom`, `Left`, `Right`

## Enabling Tooltip

Display detailed information on hover:

```razor
@using Syncfusion.Blazor.TreeMap

<SfTreeMap DataSource="@GrowthReport"
            WeightValuePath="GDP"
            RangeColorValuePath="GDP"
            TValue="Country">
    <TreeMapTitleSettings Text="Top 10 Countries by GDP Nominal - 2015">
    </TreeMapTitleSettings>
    <TreeMapLeafItemSettings LabelPath="Name">
        <TreeMapLeafColorMappings>
            <TreeMapLeafColorMapping StartRange="0" 
                                      EndRange="3000" 
                                      Color="@(new string[] { "Orange" })">
            </TreeMapLeafColorMapping>
            <TreeMapLeafColorMapping StartRange="3000" 
                                      EndRange="20000" 
                                      Color="@(new string[] { "Green" })">
            </TreeMapLeafColorMapping>
        </TreeMapLeafColorMappings>
    </TreeMapLeafItemSettings>
    <TreeMapLegendSettings Visible="true">
    </TreeMapLegendSettings>
    <TreeMapTooltipSettings Visible="true" Format="${Name} : ${GDP} Billion">
    </TreeMapTooltipSettings>
</SfTreeMap>
```

**Format Tokens:**
- `${PropertyName}` - Value from data source
- Supports custom HTML/templates

**Result:** Hovering over a country shows "Country Name : GDP Value Billion"

## License Registration

For production use, you must register a valid Syncfusion license key.

### Obtain License Key

1. **Community License** (free): [https://www.syncfusion.com/products/communitylicense](https://www.syncfusion.com/products/communitylicense)
2. **Commercial License**: Available with purchase
3. **Trial License**: 30-day trial from [Syncfusion website](https://www.syncfusion.com/downloads)

### Register License

In `Program.cs`, add license registration **before** `builder.Build()`:

```csharp
using Syncfusion.Blazor;
using Syncfusion.Licensing;

var builder = WebAssemblyHostBuilder.CreateDefault(args);
// ... other configuration

// Register Syncfusion License
Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense("YOUR_LICENSE_KEY");

builder.Services.AddSyncfusionBlazor();

await builder.Build().RunAsync();
```

**⚠️ Important:**
- Replace `YOUR_LICENSE_KEY` with actual license key
- Missing license shows warning banner in production
- License is **not** required for development/debugging

## Troubleshooting Common Issues

### Issue 1: TreeMap Not Rendering

**Symptoms:** Blank page or no TreeMap visible

**Solutions:**
1. Verify NuGet package installed: Check `.csproj` for `<PackageReference Include="Syncfusion.Blazor.TreeMap" />`
2. Check service registration: Ensure `AddSyncfusionBlazor()` is called in `Program.cs`
3. Verify script reference: Check browser console for 404 errors on `syncfusion-blazor.min.js`
4. Confirm theme CSS: Inspect page source for Syncfusion theme link

### Issue 2: "Type or namespace 'Syncfusion' could not be found"

**Solution:** Add namespace imports to `_Imports.razor`:

```razor
@using Syncfusion.Blazor
@using Syncfusion.Blazor.TreeMap
```

### Issue 3: Data Not Displaying

**Symptoms:** TreeMap renders but shows empty/no rectangles

**Solutions:**
1. Verify `DataSource` is not null or empty
2. Check `WeightValuePath` matches numeric property in data model
3. Ensure `TValue` matches data collection type
4. Confirm data property types (e.g., GDP must be numeric, not string)

### Issue 4: Render Mode Errors (Blazor Web App)

**Error:** "Cannot provide a value for property 'RenderMode' on type..."

**Solution:** Add render mode directive for Per page/component interactivity:

```razor
@rendermode InteractiveAuto
```

Or remove if interactivity location is Global.

### Issue 5: Script Errors in Browser Console

**Error:** "Syncfusion is not defined"

**Solution:** Ensure script is loaded **before** component renders:
- WebAssembly: Script in `<head>` of `index.html`
- Web App: Script at end of `<body>` in `App.razor`
- Check script path: `_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js`

### Issue 6: License Warning in Production

**Warning Banner:** "This application was built using a trial version..."

**Solution:** Register valid license key in `Program.cs`:

```csharp
SyncfusionLicenseProvider.RegisterLicense("YOUR_LICENSE_KEY");
```

### Issue 7: Build Errors After Package Installation

**Error:** "The type or namespace name 'TreeMap' does not exist in the namespace 'Syncfusion.Blazor'"

**Solutions:**
1. Clean and rebuild solution:
   ```bash
   dotnet clean
   dotnet restore
   dotnet build
   ```
2. Restart IDE (Visual Studio/VS Code)
3. Check package version compatibility with .NET SDK version

### Issue 8: Performance Issues with Large Datasets

**Symptoms:** Slow rendering or browser freeze

**Solutions:**
1. Limit initial data to 500-1000 items
2. Use hierarchical data with drill-down instead of flat data
3. Implement pagination or virtualization
4. Consider using Web App with Server render mode for better performance

## Next Steps

Now that you have a working TreeMap, explore advanced features:

1. **Data Binding** - Connect to APIs, databases, and JSON files
2. **Layout Customization** - Experiment with Squarified, Horizontal, Vertical layouts
3. **Multi-Level Hierarchies** - Create drill-down navigation with `TreeMapLevels`
4. **Advanced Color Mapping** - Use Equal, Desaturation, and Palette mappings
5. **Interactive Features** - Add selection, highlight, and custom events
6. **Export Capabilities** - Enable PDF and image export

Refer to other reference files in this skill for comprehensive feature documentation.

## Additional Resources

- **Official Documentation:** [Syncfusion Blazor TreeMap Docs](https://blazor.syncfusion.com/documentation/treemap/getting-started)
- **API Reference:** [TreeMap API Docs](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.TreeMap.html)
- **Live Demos:** [TreeMap Examples](https://blazor.syncfusion.com/demos/treemap/default-functionalities)
- **GitHub Samples:** [Blazor Getting Started Examples](https://github.com/SyncfusionExamples/Blazor-Getting-Started-Examples/tree/main/TreeMap)
- **Support:** [Syncfusion Support Portal](https://www.syncfusion.com/support)
