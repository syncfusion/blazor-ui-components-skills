# Getting Started with Blazor Chart Wizard in Server App

This guide provides complete step-by-step instructions for setting up and using the Syncfusion Blazor Chart Wizard component in a Blazor Server application.

## Table of Contents

- [Overview](#overview)
- [Prerequisites](#prerequisites)
- [Creating Blazor Server App](#creating-blazor-server-app)
  - [Using Visual Studio](#using-visual-studio)
  - [Using Visual Studio Code](#using-visual-studio-code)
  - [Using .NET CLI](#using-net-cli)
- [Installing NuGet Packages](#installing-nuget-packages)
- [Configuration Steps](#configuration-steps)
  - [Adding Namespace Imports](#adding-namespace-imports)
  - [Registering Syncfusion Blazor Service](#registering-syncfusion-blazor-service)
  - [Adding Script References](#adding-script-references)
- [Adding Chart Wizard Component](#adding-chart-wizard-component)
- [Populating Data](#populating-data)
  - [Creating Data Model](#creating-data-model)
  - [Binding DataSource](#binding-datasource)
  - [Configuring Fields](#configuring-fields)
- [Applying Themes](#applying-themes)
- [Complete Working Example](#complete-working-example)
- [Running the Application](#running-the-application)
- [Troubleshooting](#troubleshooting)

## Overview

The Blazor Chart Wizard component provides an interactive interface for creating and customizing charts in Blazor Server applications. This guide covers installation, configuration, and basic usage patterns.

## Prerequisites

Before starting, ensure you have:

- **.NET SDK 6.0 or later** - [Download here](https://dotnet.microsoft.com/download)
- **Visual Studio 2022** (or VS Code with C# extension)
- **Syncfusion Blazor license** - [Get trial license](https://www.syncfusion.com/downloads/blazor)
- Basic knowledge of Blazor and C#

Check your .NET SDK version:

```bash
dotnet --version
```

## Creating Blazor Server App

Choose your preferred development environment to create a Blazor Server application.

### Using Visual Studio

1. Open Visual Studio 2022
2. Click **Create a new project**
3. Search for **Blazor Web App** template
4. Click **Next**
5. Enter Project name: `BlazorChartWizardApp`
6. Choose location and click **Next**
7. Configure project settings:
   - **Framework**: .NET 8.0 (or later)
   - **Authentication type**: None
   - **Configure for HTTPS**: Checked
   - **Interactive render mode**: Server
   - **Interactivity location**: Global
8. Click **Create**

![Blazor Server App Creation](../images/Blazor-server-app-creation.png)

### Using Visual Studio Code

1. Open VS Code
2. Press <kbd>Ctrl</kbd>+<kbd>`</kbd> to open integrated terminal
3. Run the following commands:

```bash
dotnet new blazor -o BlazorChartWizardApp -int Server
cd BlazorChartWizardApp
code .
```

This creates a Blazor Server app and opens it in VS Code.

### Using .NET CLI

Open a terminal (Command Prompt, PowerShell, or Bash) and run:

```bash
dotnet new blazor -o BlazorChartWizardApp -int Server
cd BlazorChartWizardApp
```

## Installing NuGet Packages

Install the Syncfusion Blazor Chart Wizard package using your preferred method.

### Visual Studio

1. Go to **Tools** → **NuGet Package Manager** → **Manage NuGet Packages for Solution**
2. Click **Browse** tab
3. Search for `Syncfusion.Blazor.ChartWizard`
4. Select the package
5. Check your project
6. Click **Install**
7. Accept the license terms

### Package Manager Console

In Visual Studio, open **Tools** → **NuGet Package Manager** → **Package Manager Console** and run:

```powershell
Install-Package Syncfusion.Blazor.ChartWizard -Version 27.1.48
```

*Replace version number with the latest available version.*

### .NET CLI

In your terminal (VS Code or command line), run:

```bash
dotnet add package Syncfusion.Blazor.ChartWizard
dotnet restore
```

## Configuration Steps

After installing the package, configure your Blazor Server app to use the Chart Wizard component.

### Adding Namespace Imports

Open the `_Imports.razor` file (located in the project root) and add these namespaces:

```cshtml
@using Syncfusion.Blazor
@using Syncfusion.Blazor.ChartWizard
```

This makes Syncfusion Blazor components available throughout your application without needing to add `@using` statements to every page.

### Registering Syncfusion Blazor Service

Open `Program.cs` and register the Syncfusion Blazor service:

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

**Key line:** `builder.Services.AddSyncfusionBlazor();`

### Adding Script References

Open `App.razor` (or `_Host.cshtml` in older Blazor Server apps) and add the Syncfusion script reference at the end of the `<body>` tag:

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <base href="/" />
    <link rel="stylesheet" href="bootstrap/bootstrap.min.css" />
    <link rel="stylesheet" href="app.css" />
    <link rel="stylesheet" href="BlazorChartWizardApp.styles.css" />
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

**Important:** The script reference must come after `blazor.web.js`.

## Adding Chart Wizard Component

Now you can add the Chart Wizard component to your Blazor pages.

### Basic Component Declaration

Open `Components/Pages/Home.razor` and replace the content with:

```cshtml
@page "/"
@rendermode InteractiveServer

<PageTitle>Chart Wizard</PageTitle>

<h1>Blazor Chart Wizard</h1>

<SfChartWizard>
    
</SfChartWizard>
```

**Note:** If your interactivity location is set to "Per page/component", you must specify `@rendermode InteractiveServer` at the top of the page.

This creates an empty Chart Wizard component. Next, we'll add data.

## Populating Data

To display a chart, you need to provide data and configure how it should be visualized.

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

Create a data collection:

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

### Configuring Fields

Bind the data to the Chart Wizard and configure which fields to use:

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
    
    // ... rest of code
}
```

**Explanation:**

- **DataSource**: The data collection to visualize
- **CategoryFields**: Properties to use for x-axis categories (Country, CountryCode)
- **SeriesFields**: Properties to use for data series (Gold, Silver, Bronze)
- **SeriesType**: Type of chart to display (Column, Bar, Line, Area, Pie, etc.)

**Note:** The default series type is `Line`. Use the `SeriesType` property to change it to Column, Bar, Area, or other supported types.

## Applying Themes

The Chart Wizard supports multiple built-in themes for consistent styling.

### Including Theme Stylesheet

Add the theme stylesheet reference in the `<head>` section of `App.razor`:

```html
<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <base href="/" />
    <link rel="stylesheet" href="bootstrap/bootstrap.min.css" />
    <link rel="stylesheet" href="app.css" />
    
    <!-- Add Syncfusion theme stylesheet -->
    <link href="_content/Syncfusion.Blazor.Themes/material3.css" rel="stylesheet" />
    
    <link rel="icon" type="image/png" href="favicon.png" />
    <HeadOutlet />
</head>
```

**Available themes:**
- `material3.css` - Material 3 theme
- `fluent2.css` - Fluent 2 theme
- `bootstrap5.css` - Bootstrap 5 theme
- `tailwind3.css` - Tailwind 3 theme
- `fabric.css` - Fabric theme

### Applying Theme to Chart Wizard

Set the `Theme` property on the Chart Wizard component:

```cshtml
<SfChartWizard Theme="Theme.Material3">
    <ChartSettings DataSource="@OlympicsDataSource"
                   CategoryFields="@categories"
                   SeriesType="ChartWizardSeriesType.Bar"
                   SeriesFields="@chartSeries">
    </ChartSettings>
</SfChartWizard>
```

**Important:** The theme stylesheet in `<head>` affects the overall Chart Wizard UI, while the `Theme` property specifically styles the chart visualization area.

## Complete Working Example

Here's the complete `Home.razor` file with all components integrated:

```cshtml
@page "/"
@rendermode InteractiveServer

<PageTitle>Chart Wizard</PageTitle>

<div class="container mt-4">
    <h1>Olympic Medals Chart</h1>
    <p>Interactive visualization of Olympic medal counts by country</p>
    
    <SfChartWizard Theme="Theme.Material3">
        <ChartSettings DataSource="@OlympicsDataSource"
                       CategoryFields="@categories"
                       SeriesType="ChartWizardSeriesType.Bar"
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

Press <kbd>F5</kbd> or <kbd>Ctrl</kbd>+<kbd>F5</kbd> to run the application.

### VS Code / .NET CLI

In the terminal, run:

```bash
dotnet run
```

Then open your browser to the URL shown (typically `https://localhost:5001` or `http://localhost:5000`).

You should see the Chart Wizard component displaying Olympic medal data in a bar chart with an interactive property panel for customization.

![Chart Wizard Example](../images/chart-wizard-getting-started-example.png)

## Troubleshooting

### Chart Not Displaying

**Problem:** The Chart Wizard component renders but the chart doesn't display.

**Solutions:**

1. **Check DataSource binding:** Ensure `DataSource="@OlympicsDataSource"` references a valid data collection
2. **Verify field names:** CategoryFields and SeriesFields must match property names in your data model (case-sensitive)
3. **Check browser console:** Press F12 and look for JavaScript errors
4. **Verify script reference:** Ensure `syncfusion-blazor.min.js` is loaded after `blazor.web.js`

### Script Reference Errors

**Problem:** Console shows "Syncfusion is not defined" or similar errors.

**Solutions:**

1. **Check script order:** Syncfusion script must come after `blazor.web.js`
2. **Verify package installation:** Run `dotnet restore` to ensure packages are properly installed
3. **Check path:** Script path should be `_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js`
4. **Clear browser cache:** Sometimes old cached scripts cause issues

### Theme Not Applying

**Problem:** Chart Wizard displays but theme doesn't look right.

**Solutions:**

1. **Check stylesheet reference:** Ensure theme CSS is added in `<head>` section of App.razor
2. **Verify Theme property:** Match `Theme` property value to stylesheet (e.g., `Theme.Material3` with `material3.css`)
3. **Check file path:** Stylesheet path should be `_content/Syncfusion.Blazor.Themes/[theme].css`
4. **Hard refresh browser:** Press <kbd>Ctrl</kbd>+<kbd>F5</kbd> to clear CSS cache

### Render Mode Issues

**Problem:** Component doesn't render or shows "No render mode" error.

**Solutions:**

1. **Add render mode:** Include `@rendermode InteractiveServer` at the top of your page
2. **Check Program.cs:** Ensure `.AddInteractiveServerComponents()` is called
3. **Verify interactivity location:** Match your render mode to project configuration

### License Errors

**Problem:** "License key not found" message appears.

**Solutions:**

1. **Register license:** Add `Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense("YOUR_LICENSE_KEY");` in Program.cs before `builder.Build()`
2. **Get trial license:** Visit [Syncfusion website](https://www.syncfusion.com/downloads/blazor) to get a free trial license
3. **Check license validity:** Ensure your license hasn't expired

### Data Not Updating

**Problem:** Chart doesn't update when data changes.

**Solutions:**

1. **Use ObservableCollection:** For dynamic data, use `ObservableCollection<T>` instead of `List<T>`
2. **Call StateHasChanged():** Manually trigger re-render after data changes
3. **Check data binding:** Ensure data properties have getters and setters

## Next Steps

Now that you have a working Chart Wizard in your Blazor Server app:

1. **Explore chart types:** Try different `SeriesType` values (Line, Area, Pie, Scatter, etc.)
2. **Customize appearance:** Add Width, Height, and PropertyPanelExpanded properties
3. **Add export functionality:** Configure ChartExportSettings for saving charts
4. **Implement serialization:** Use SaveChart() and LoadChartAsync() for persistence
5. **Enable accessibility:** Test keyboard navigation and screen reader support

For more advanced scenarios, explore the other reference guides for data binding, customization, export, and accessibility features.
