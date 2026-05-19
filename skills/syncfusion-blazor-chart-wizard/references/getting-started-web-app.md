# Getting Started with Blazor Chart Wizard in Web App

This guide provides complete step-by-step instructions for setting up and using the Syncfusion Blazor Chart Wizard component in a Blazor Web App with support for multiple render modes (Server, WebAssembly, and Auto).

## Table of Contents

- [Overview](#overview)
- [Prerequisites](#prerequisites)
- [Understanding Render Modes](#understanding-render-modes)
  - [Interactive Render Modes](#interactive-render-modes)
  - [Interactivity Location](#interactivity-location)
  - [Choosing the Right Mode](#choosing-the-right-mode)
- [Creating Blazor Web App](#creating-blazor-web-app)
  - [Using Visual Studio](#using-visual-studio)
  - [Using Visual Studio Code](#using-visual-studio-code)
  - [Using .NET CLI](#using-net-cli)
- [Installing NuGet Packages](#installing-nuget-packages)
  - [Server Render Mode](#server-render-mode)
  - [WebAssembly or Auto Render Mode](#webassembly-or-auto-render-mode)
- [Configuration Steps](#configuration-steps)
  - [Adding Namespace Imports](#adding-namespace-imports)
  - [Registering Syncfusion Blazor Service](#registering-syncfusion-blazor-service)
  - [Adding Script References](#adding-script-references)
- [Adding Chart Wizard Component](#adding-chart-wizard-component)
  - [Per Page/Component Interactivity](#per-pagecomponent-interactivity)
  - [Global Interactivity](#global-interactivity)
- [Populating Data](#populating-data)
  - [Creating Data Model](#creating-data-model)
  - [Binding DataSource](#binding-datasource)
  - [Configuring Chart Fields](#configuring-chart-fields)
- [Applying Themes](#applying-themes)
- [Complete Working Examples](#complete-working-examples)
  - [Server Mode Example](#server-mode-example)
  - [WebAssembly Mode Example](#webassembly-mode-example)
  - [Auto Mode Example](#auto-mode-example)
- [Running the Application](#running-the-application)
- [Render Mode Best Practices](#render-mode-best-practices)
- [Troubleshooting](#troubleshooting)

## Overview

Blazor Web App is a unified hosting model introduced in .NET 8+ that supports multiple render modes in a single application. This allows you to choose the most appropriate rendering strategy for different parts of your application, combining the benefits of Server-side rendering, Client-side WebAssembly, and automatic switching between modes.

The Chart Wizard component works seamlessly with all render modes, providing an interactive interface for creating and customizing charts.

## Prerequisites

Before starting, ensure you have:

- **.NET SDK 8.0 or later** - [Download here](https://dotnet.microsoft.com/download)
- **Visual Studio 2022 version 17.8+** (or VS Code with C# Dev Kit extension)
- **Syncfusion Blazor license** - [Get trial license](https://www.syncfusion.com/downloads/blazor)
- Basic understanding of Blazor render modes

Verify your .NET SDK version:

```bash
dotnet --version
```

**System Requirements:**
- For development: Windows 10+, macOS 10.15+, or Linux
- For hosting: IIS 10+, Kestrel, nginx, or Apache

## Understanding Render Modes

Blazor Web App supports multiple render modes that determine where and how components are rendered.

### Interactive Render Modes

**1. Server Mode (InteractiveServer):**
- Components run on the server
- UI updates sent via SignalR connection
- Lower initial download size
- Requires persistent connection
- Best for: Internal apps, complex business logic

**2. WebAssembly Mode (InteractiveWebAssembly):**
- Components run in the browser using WebAssembly
- No server connection needed after initial load
- Larger initial download
- Offline capability
- Best for: Public apps, offline scenarios

**3. Auto Mode (InteractiveAuto):**
- Initially uses Server mode for fast startup
- Downloads WebAssembly in background
- Switches to WebAssembly on subsequent visits
- Best of both worlds
- Best for: Production apps needing optimal performance

**4. Static Server Rendering (None):**
- No interactivity
- HTML rendered on server
- No real-time updates
- Not suitable for Chart Wizard (requires interactivity)

### Interactivity Location

**Global:**
- Render mode set in `App.razor`
- Applies to all interactive components
- Simpler configuration
- Consistent behavior across app

**Per Page/Component:**
- Render mode specified with `@rendermode` directive
- Different modes for different components
- More flexibility
- Requires explicit configuration on each component

### Choosing the Right Mode

| Scenario | Recommended Mode | Reason |
|----------|------------------|---------|
| Internal dashboard | Server | Fast load, server resources available |
| Public website | WebAssembly | No server dependency, scalable |
| Enterprise app | Auto | Best initial experience + offline capability |
| Mixed requirements | Per Page/Component | Use Server for some pages, WASM for others |

## Creating Blazor Web App

Choose your preferred development environment to create a Blazor Web App.

### Using Visual Studio

1. Open Visual Studio 2022 (version 17.8 or later)
2. Click **Create a new project**
3. Search for **Blazor Web App** template
4. Select it and click **Next**
5. Enter Project name: `BlazorChartWizardWebApp`
6. Choose location and click **Next**
7. Configure project settings:
   - **Framework**: .NET 8.0 or later
   - **Authentication type**: None
   - **Configure for HTTPS**: Checked (recommended)
   - **Interactive render mode**: Choose Server, WebAssembly, or Auto
   - **Interactivity location**: Choose Global or Per page/component
   - **Include sample pages**: Optional (unchecked for clean start)
8. Click **Create**

![Create Blazor Web App](../images/blazor-create-web-app.png)

**Note:** If you select WebAssembly or Auto mode, Visual Studio creates a solution with two projects:
- `BlazorChartWizardWebApp` (Server project)
- `BlazorChartWizardWebApp.Client` (Client project for WASM components)

### Using Visual Studio Code

1. Open VS Code
2. Press <kbd>Ctrl</kbd>+<kbd>`</kbd> to open integrated terminal
3. Create a Blazor Web App with your chosen render mode:

**For Server render mode:**
```bash
dotnet new blazor -o BlazorChartWizardWebApp -int Server
cd BlazorChartWizardWebApp
code .
```

**For WebAssembly render mode:**
```bash
dotnet new blazor -o BlazorChartWizardWebApp -int WebAssembly
cd BlazorChartWizardWebApp
code .
```

**For Auto render mode (recommended):**
```bash
dotnet new blazor -o BlazorChartWizardWebApp -int Auto
cd BlazorChartWizardWebApp
code .
```

The `-int` parameter specifies the interactive render mode.

### Using .NET CLI

Open a terminal (Command Prompt, PowerShell, or Bash) and create the app with your preferred render mode:

**Auto render mode (recommended):**
```bash
dotnet new blazor -o BlazorChartWizardWebApp -int Auto
cd BlazorChartWizardWebApp
```

**Server render mode:**
```bash
dotnet new blazor -o BlazorChartWizardWebApp -int Server
cd BlazorChartWizardWebApp
```

**WebAssembly render mode:**
```bash
dotnet new blazor -o BlazorChartWizardWebApp -int WebAssembly
cd BlazorChartWizardWebApp
```

## Installing NuGet Packages

The installation process differs based on your chosen render mode.

### Server Render Mode

For Server-only mode, install the package in the main project.

**Using Visual Studio:**
1. Go to **Tools** → **NuGet Package Manager** → **Manage NuGet Packages for Solution**
2. Click **Browse** tab
3. Search for `Syncfusion.Blazor.ChartWizard`
4. Select the package
5. Check your project (main project only)
6. Click **Install**

**Using Package Manager Console:**
```powershell
Install-Package Syncfusion.Blazor.ChartWizard -Version 27.1.48
```

**Using .NET CLI:**
```bash
dotnet add package Syncfusion.Blazor.ChartWizard
dotnet restore
```

### WebAssembly or Auto Render Mode

For WebAssembly or Auto modes, install the package in the **Client** project.

**Important:** With WebAssembly/Auto modes, your solution has two projects:
- `BlazorChartWizardWebApp` (Server project)
- `BlazorChartWizardWebApp.Client` (Client project)

Install the NuGet package in the **Client** project.

**Using Visual Studio:**
1. In Solution Explorer, right-click on `BlazorChartWizardWebApp.Client`
2. Select **Manage NuGet Packages**
3. Click **Browse** tab
4. Search for `Syncfusion.Blazor.ChartWizard`
5. Click **Install**

**Using Package Manager Console:**
```powershell
# Navigate to Client project first
cd BlazorChartWizardWebApp.Client
Install-Package Syncfusion.Blazor.ChartWizard -Version 27.1.48
```

**Using .NET CLI:**
```bash
cd BlazorChartWizardWebApp.Client
dotnet add package Syncfusion.Blazor.ChartWizard
dotnet restore
cd ..
```

## Configuration Steps

Configuration varies based on your render mode. Follow the appropriate section.

### Adding Namespace Imports

**For Server Mode:**
Open `_Imports.razor` in the main project and add:

```cshtml
@using Syncfusion.Blazor
@using Syncfusion.Blazor.ChartWizard
```

**For WebAssembly or Auto Mode:**
Open `_Imports.razor` in the **Client** project (`BlazorChartWizardWebApp.Client/_Imports.razor`) and add:

```cshtml
@using Syncfusion.Blazor
@using Syncfusion.Blazor.ChartWizard
```

### Registering Syncfusion Blazor Service

The service registration differs significantly based on render mode.

**For Server Mode:**
Register the service in the main `Program.cs`:

```csharp
using Syncfusion.Blazor;

var builder = WebApplication.CreateBuilder(args);

// Add services to the container.
builder.Services.AddRazorComponents()
    .AddInteractiveServerComponents();

// Register Syncfusion Blazor service
builder.Services.AddSyncfusionBlazor();

var app = builder.Build();

// Configure the HTTP request pipeline.
if (!app.Environment.IsDevelopment())
{
    app.UseExceptionHandler("/Error", createScopeForErrors: true);
    app.UseHsts();
}

app.UseHttpsRedirection();
app.UseStaticFiles();
app.UseAntiforgery();

app.MapRazorComponents<App>()
    .AddInteractiveServerRenderMode();

app.Run();
```

**For WebAssembly or Auto Mode:**
Register the service in **BOTH** the server and client `Program.cs` files.

**Server Program.cs** (`BlazorChartWizardWebApp/Program.cs`):

```csharp
using Syncfusion.Blazor;

var builder = WebApplication.CreateBuilder(args);

// Add services to the container.
builder.Services.AddRazorComponents()
    .AddInteractiveServerComponents()
    .AddInteractiveWebAssemblyComponents();

// Register Syncfusion Blazor service for server components
builder.Services.AddSyncfusionBlazor();

var app = builder.Build();

// Configure the HTTP request pipeline.
if (!app.Environment.IsDevelopment())
{
    app.UseExceptionHandler("/Error", createScopeForErrors: true);
    app.UseHsts();
}

app.UseHttpsRedirection();
app.UseStaticFiles();
app.UseAntiforgery();

app.MapRazorComponents<App>()
    .AddInteractiveServerRenderMode()
    .AddInteractiveWebAssemblyRenderMode()
    .AddAdditionalAssemblies(typeof(BlazorChartWizardWebApp.Client._Imports).Assembly);

app.Run();
```

**Client Program.cs** (`BlazorChartWizardWebApp.Client/Program.cs`):

```csharp
using Microsoft.AspNetCore.Components.WebAssembly.Hosting;
using Syncfusion.Blazor;

var builder = WebAssemblyHostBuilder.CreateDefault(args);

// Register Syncfusion Blazor service for client components
builder.Services.AddSyncfusionBlazor();

await builder.Build().RunAsync();
```

**Optional - License Registration:**
If you have a Syncfusion license key, register it in both files before `AddSyncfusionBlazor()`:

```csharp
Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense("YOUR_LICENSE_KEY");
builder.Services.AddSyncfusionBlazor();
```

### Adding Script References

Add the Syncfusion script reference in `Components/App.razor` at the end of the `<body>` tag:

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <base href="/" />
    <link rel="stylesheet" href="bootstrap/bootstrap.min.css" />
    <link rel="stylesheet" href="app.css" />
    <link rel="stylesheet" href="BlazorChartWizardWebApp.styles.css" />
    <link rel="icon" type="image/png" href="favicon.png" />
    <HeadOutlet />
</head>

<body>
    <Routes />
    <script src="_framework/blazor.web.js"></script>
    
    <!-- Add Syncfusion Blazor script reference -->
    <script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js" type="text/javascript"></script>
</body>

</html>
```

**Critical:** The Syncfusion script must be placed after `blazor.web.js`.

## Adding Chart Wizard Component

How you add the component depends on your interactivity location setting.

### Per Page/Component Interactivity

If you chose **Per page/component** interactivity location during project creation, you must specify the render mode on each page.

Open `Components/Pages/Home.razor` and add the render mode directive:

**For Server mode:**
```cshtml
@page "/"
@rendermode InteractiveServer

<PageTitle>Chart Wizard</PageTitle>

<h1>Blazor Chart Wizard - Server Mode</h1>

<SfChartWizard>
    
</SfChartWizard>
```

**For WebAssembly mode:**
```cshtml
@page "/"
@rendermode InteractiveWebAssembly

<PageTitle>Chart Wizard</PageTitle>

<h1>Blazor Chart Wizard - WASM Mode</h1>

<SfChartWizard>
    
</SfChartWizard>
```

**For Auto mode:**
```cshtml
@page "/"
@rendermode InteractiveAuto

<PageTitle>Chart Wizard</PageTitle>

<h1>Blazor Chart Wizard - Auto Mode</h1>

<SfChartWizard>
    
</SfChartWizard>
```

### Global Interactivity

If you chose **Global** interactivity location, the render mode is already configured in `App.razor`, and you don't need to add `@rendermode` to individual pages.

Simply add the component:

```cshtml
@page "/"

<PageTitle>Chart Wizard</PageTitle>

<h1>Blazor Chart Wizard</h1>

<SfChartWizard>
    
</SfChartWizard>
```

**Note:** For WebAssembly or Auto modes with Global interactivity, place your components in the **Client** project under `Components/Pages/` directory.

## Populating Data

To display a chart, you need to provide data and configure the visualization.

### Creating Data Model

Add a data model class in your code block:

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

### Binding DataSource

Create a sample data collection:

```csharp
@code {
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
}
```

### Configuring Chart Fields

Bind the data to the Chart Wizard and specify which fields to use:

```cshtml
<SfChartWizard>
    <ChartSettings DataSource="@OlympicsDataSource"
                   CategoryFields="@categories"
                   SeriesType="ChartWizardSeriesType.Column"
                   SeriesFields="@chartSeries">
    </ChartSettings>
</SfChartWizard>

@code {
    private readonly List<string> chartSeries = new() { "Gold", "Silver", "Bronze" };
    private readonly List<string> categories = new() { "Country", "CountryCode" };
    
    // ... data model and collection here
}
```

**Properties Explained:**

- **DataSource**: The data collection to visualize
- **CategoryFields**: Properties for x-axis categories (can select from multiple options)
- **SeriesFields**: Properties for data series (Gold, Silver, Bronze values)
- **SeriesType**: Chart type (Column, Bar, Line, Area, Pie, Doughnut, etc.)

**Note:** The default series type is `Line`. Use the `SeriesType` property to change to other chart types.

## Applying Themes

The Chart Wizard supports multiple built-in themes for consistent styling across render modes.

### Adding Theme Stylesheet

Add the theme stylesheet in the `<head>` section of `Components/App.razor`:

```html
<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <base href="/" />
    <link rel="stylesheet" href="bootstrap/bootstrap.min.css" />
    <link rel="stylesheet" href="app.css" />
    
    <!-- Add Syncfusion theme stylesheet -->
    <link href="_content/Syncfusion.Blazor.Themes/material3.css" rel="stylesheet" />
    
    <link rel="stylesheet" href="BlazorChartWizardWebApp.styles.css" />
    <link rel="icon" type="image/png" href="favicon.png" />
    <HeadOutlet />
</head>
```

**Available Themes:**
- Material: `material3.css`, `material3-dark.css`
- Fluent: `fluent2.css`, `fluent2-dark.css`, `fluent2-highcontrast.css`
- Bootstrap: `bootstrap5.css`, `bootstrap5-dark.css`
- Tailwind: `tailwind3.css`, `tailwind3-dark.css`
- Fabric: `fabric.css`, `fabric-dark.css`

### Applying Theme to Component

Set the `Theme` property on the Chart Wizard:

```cshtml
<SfChartWizard Theme="Theme.Material3">
    <ChartSettings DataSource="@OlympicsDataSource"
                   CategoryFields="@categories"
                   SeriesType="ChartWizardSeriesType.Bar"
                   SeriesFields="@chartSeries">
    </ChartSettings>
</SfChartWizard>
```

**Important:** The theme stylesheet must match the Theme property:
- `material3.css` → `Theme.Material3`
- `fluent2.css` → `Theme.Fluent2`
- `bootstrap5.css` → `Theme.Bootstrap5`

The stylesheet affects the overall UI, while the Theme property styles the chart area.

## Complete Working Examples

Here are complete examples for each render mode.

### Server Mode Example

**File:** `Components/Pages/Home.razor`

```cshtml
@page "/"
@rendermode InteractiveServer

<PageTitle>Chart Wizard - Server Mode</PageTitle>

<div class="container mt-4">
    <h1>Olympic Medals Chart (Server Mode)</h1>
    <p>Interactive chart with server-side rendering and SignalR updates</p>
    
    <SfChartWizard Theme="Theme.Material3" Width="100%" Height="600px">
        <ChartSettings DataSource="@OlympicsDataSource"
                       CategoryFields="@categories"
                       SeriesType="ChartWizardSeriesType.Column"
                       SeriesFields="@chartSeries">
        </ChartSettings>
    </SfChartWizard>
</div>

@code {
    private readonly List<string> chartSeries = new() { "Gold", "Silver", "Bronze" };
    private readonly List<string> categories = new() { "Country", "CountryCode" };

    public class OlympicsData
    {
        public string? Country { get; set; }
        public string? CountryCode { get; set; }
        public int Gold { get; set; }
        public int Silver { get; set; }
        public int Bronze { get; set; }
    }

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
}
```

### WebAssembly Mode Example

**File:** `BlazorChartWizardWebApp.Client/Components/Pages/Home.razor`

```cshtml
@page "/"
@rendermode InteractiveWebAssembly

<PageTitle>Chart Wizard - WASM Mode</PageTitle>

<div class="container mt-4">
    <h1>Olympic Medals Chart (WebAssembly Mode)</h1>
    <p>Running entirely in your browser with WebAssembly</p>
    
    <SfChartWizard Theme="Theme.Fluent2" Width="100%" Height="600px">
        <ChartSettings DataSource="@OlympicsDataSource"
                       CategoryFields="@categories"
                       SeriesType="ChartWizardSeriesType.Bar"
                       SeriesFields="@chartSeries">
        </ChartSettings>
    </SfChartWizard>
</div>

@code {
    private readonly List<string> chartSeries = new() { "Gold", "Silver", "Bronze" };
    private readonly List<string> categories = new() { "Country" };

    public class OlympicsData
    {
        public string? Country { get; set; }
        public string? CountryCode { get; set; }
        public int Gold { get; set; }
        public int Silver { get; set; }
        public int Bronze { get; set; }
    }

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
}
```

### Auto Mode Example

**File:** `BlazorChartWizardWebApp.Client/Components/Pages/Home.razor`

```cshtml
@page "/"
@rendermode InteractiveAuto

<PageTitle>Chart Wizard - Auto Mode</PageTitle>

<div class="container mt-4">
    <h1>Olympic Medals Chart (Auto Mode)</h1>
    <p>Optimized for best performance - Server initially, then WebAssembly</p>
    
    <SfChartWizard Theme="Theme.Bootstrap5" Width="100%" Height="600px">
        <ChartSettings DataSource="@OlympicsDataSource"
                       CategoryFields="@categories"
                       SeriesType="ChartWizardSeriesType.Line"
                       SeriesFields="@chartSeries">
        </ChartSettings>
    </SfChartWizard>
</div>

@code {
    private readonly List<string> chartSeries = new() { "Gold", "Silver", "Bronze" };
    private readonly List<string> categories = new() { "Country", "CountryCode" };

    public class OlympicsData
    {
        public string? Country { get; set; }
        public string? CountryCode { get; set; }
        public int Gold { get; set; }
        public int Silver { get; set; }
        public int Bronze { get; set; }
    }

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
}
```

## Running the Application

### Visual Studio

Press <kbd>F5</kbd> or <kbd>Ctrl</kbd>+<kbd>F5</kbd> to launch the application.

**First Launch Experience by Render Mode:**
- **Server**: Instant load, requires SignalR connection
- **WebAssembly**: 3-5 second initial load (downloading WASM runtime)
- **Auto**: Fast Server rendering initially, WASM downloaded in background

### VS Code / .NET CLI

In the terminal, navigate to the main project directory and run:

```bash
dotnet run
```

Or for better development experience with hot reload:

```bash
dotnet watch
```

Then open your browser to the displayed URL (typically `https://localhost:5001` or `http://localhost:5000`).

You should see the Chart Wizard component displaying Olympic medal data with an interactive property panel for customization.

![Chart Wizard Example](../images/chart-wizard-getting-started-example.png)

## Render Mode Best Practices

### Package Management

**Server Mode:**
- Install packages in server project only
- Smaller package footprint
- All dependencies stay on server

**WebAssembly/Auto Mode:**
- Install packages in Client project
- Packages downloaded to browser
- Consider package size impact

### Component Placement

**Server Mode:**
- Place components in server project under `Components/Pages/`
- Components can access server-side resources directly

**WebAssembly/Auto Mode:**
- Place interactive components in Client project
- Use dependency injection for API calls
- Avoid direct database access

### Performance Considerations

**Server Mode Advantages:**
- No download wait time
- Access to server resources
- Smaller bandwidth requirements

**Server Mode Challenges:**
- Requires persistent connection
- SignalR overhead
- Server CPU/memory usage per user

**WebAssembly Mode Advantages:**
- No server dependency after load
- Offline capability
- Reduces server load

**WebAssembly Mode Challenges:**
- Larger initial download (5-15MB)
- Longer first load time
- Limited to client-side operations

**Auto Mode Benefits:**
- Best of both worlds
- Fast initial load (Server)
- Offline capability after first visit (WASM)
- Recommended for production

### Switching Between Modes

You can mix render modes in a single app using per-page/component interactivity:

```cshtml
@* Some pages use Server mode *@
@page "/dashboard"
@rendermode InteractiveServer

@* Other pages use WebAssembly mode *@
@page "/offline-capable"
@rendermode InteractiveWebAssembly
```

**Best Practice:** Use Server mode for internal/admin pages, WebAssembly for public-facing features.

## Troubleshooting

### Render Mode Not Working

**Problem:** Component doesn't render or shows "No render mode" error.

**Solutions:**

1. **Check @rendermode directive:** Ensure it's added for per-page/component interactivity
2. **Verify Program.cs registration:** 
   - Server mode: `.AddInteractiveServerRenderMode()`
   - WASM mode: `.AddInteractiveWebAssemblyRenderMode()`
   - Auto mode: Both methods called
3. **Check project references:** For WASM/Auto, ensure `AddAdditionalAssemblies()` includes Client assembly
4. **Verify interactivity location:** Match your configuration to project setup

### Package Not Found in Client Project

**Problem:** "Type or namespace 'Syncfusion' could not be found" in Client project.

**Solutions:**

1. **Install in correct project:** Ensure NuGet package installed in `.Client` project for WASM/Auto modes
2. **Check _Imports.razor:** Verify `@using Syncfusion.Blazor` in Client project
3. **Rebuild solution:** Clean and rebuild both projects
4. **Verify project references:** Check Client project has proper dependencies

### SignalR Connection Issues (Server Mode)

**Problem:** Chart not updating, SignalR connection errors in console.

**Solutions:**

1. **Check HTTPS configuration:** Ensure proper SSL/TLS setup
2. **Firewall rules:** Allow SignalR WebSocket connections
3. **Browser compatibility:** Test in different browsers
4. **Check network:** Verify no proxy/firewall blocking WebSockets

### WASM Loading Errors

**Problem:** "Failed to fetch" or 404 errors loading WASM resources.

**Solutions:**

1. **Check web.config:** Ensure proper MIME types configured:
```xml
<staticContent>
    <mimeMap fileExtension=".blat" mimeType="application/octet-stream" />
    <mimeMap fileExtension=".dat" mimeType="application/octet-stream" />
</staticContent>
```
2. **Verify build output:** Check `_framework` folder exists in `wwwroot`
3. **Clear browser cache:** Hard refresh with <kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>R</kbd>
4. **Check server configuration:** Ensure static files middleware enabled

### Auto Mode Not Switching to WASM

**Problem:** Auto mode stays in Server mode on subsequent visits.

**Solutions:**

1. **Check browser storage:** WASM assemblies cached in browser
2. **Verify both Program.cs files:** Service registered in both server and client
3. **Network speed:** Slow connections may delay WASM download
4. **Check download size:** Large apps take longer to download
5. **Browser DevTools:** Check Network tab for WASM assembly downloads

### Script Reference Errors

**Problem:** "Syncfusion is not defined" in browser console.

**Solutions:**

1. **Verify script order:** Syncfusion script must come after `blazor.web.js`
2. **Check App.razor:** Script reference in `<body>` not `<head>`
3. **Clear cache:** Browser may have cached old version
4. **Verify path:** Should be `_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js`

### Theme Not Applying

**Problem:** Chart displays but styling is incorrect.

**Solutions:**

1. **Check stylesheet location:** Must be in `<head>` of App.razor
2. **Match theme property:** `Theme.Material3` requires `material3.css`
3. **Verify file path:** `_content/Syncfusion.Blazor.Themes/[theme].css`
4. **Clear cache:** Browser may have cached old styles

### Data Not Displaying

**Problem:** Chart Wizard loads but no data shown.

**Solutions:**

1. **Check DataSource binding:** Verify `DataSource="@OlympicsDataSource"` syntax
2. **Verify field names:** CategoryFields/SeriesFields must match property names (case-sensitive)
3. **Check data collection:** Ensure list has data in `OnInitialized()`
4. **Browser console:** Look for JavaScript errors
5. **Inspect component:** Use browser DevTools to check rendered HTML

### Performance Issues with Large Data

**Problem:** Chart is slow with large datasets.

**Solutions:**

1. **Limit data points:** Show only necessary data
2. **Use pagination:** Load data in chunks
3. **Consider Server mode:** Better for large server-side datasets
4. **Disable animations:** Set animation properties to false
5. **Use data virtualization:** For very large datasets

## Next Steps

Now that you have a working Chart Wizard in your Blazor Web App:

1. **Experiment with render modes:** Try Server, WebAssembly, and Auto to see differences
2. **Optimize for production:** Consider Auto mode for best user experience
3. **Explore chart types:** Try different SeriesType values (Pie, Area, Scatter, etc.)
4. **Add export functionality:** Configure ChartExportSettings for saving charts
5. **Implement data persistence:** Use serialization for saving/loading chart configurations
6. **Enable accessibility features:** Test keyboard navigation and screen readers
7. **Mix render modes:** Use appropriate modes for different pages

For advanced scenarios, explore the other reference guides for data binding, customization, export, serialization, and accessibility features.
