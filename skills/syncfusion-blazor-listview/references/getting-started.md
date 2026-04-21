# Getting Started with Syncfusion Blazor ListView

## Table of Contents
- [Prerequisites](#prerequisites)
- [Installation Steps](#installation-steps)
- [Setup for Blazor WebAssembly](#setup-for-blazor-webassembly)
- [Setup for Blazor Web App](#setup-for-blazor-web-app)
- [Setup for Blazor Server App](#setup-for-blazor-server-app)
- [Initial Component Setup](#initial-component-setup)
- [Complete Example](#complete-example)

## Prerequisites

- .NET SDK (latest version)
- Visual Studio 2022, Visual Studio Code, or .NET CLI
- Basic knowledge of Blazor and C#

Check system requirements for Blazor components at [Syncfusion documentation](url).

## Installation Steps

### 1. Create a Blazor Project

**Via Visual Studio:**
- Create new project → Blazor WebAssembly/Blazor Web App/Blazor Server App
- Or use Syncfusion Blazor Extension for template support

**Via .NET CLI:**
```bash
# WebAssembly
dotnet new blazorwasm -o BlazorApp
cd BlazorApp

# Web App (Auto render mode)
dotnet new blazor -o BlazorWebApp -int Auto
cd BlazorWebApp/BlazorWebApp.Client

# Server
dotnet new blazor -o BlazorApp -int Server
cd BlazorApp
```

### 2. Install NuGet Packages

**Using Package Manager Console:**
```powershell
Install-Package Syncfusion.Blazor.Lists -Version {{ site.releaseversion }}
Install-Package Syncfusion.Blazor.Themes -Version {{ site.releaseversion }}
```

**Using .NET CLI:**
```bash
dotnet add package Syncfusion.Blazor.Lists -Version {{ site.releaseversion }}
dotnet add package Syncfusion.Blazor.Themes -Version {{ site.releaseversion }}
dotnet restore
```

### 3. Add Import Namespaces

Open `~/_Imports.razor` and add:

```csharp
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Lists
```

### 4. Register Syncfusion Service

Open `~/Program.cs` and register the Syncfusion Blazor service:

```csharp
using Syncfusion.Blazor;

// WebAssembly
var builder = WebAssemblyHostBuilder.CreateDefault(args);
builder.Services.AddScoped(sp => new HttpClient { BaseAddress = new Uri(builder.HostEnvironment.BaseAddress) });
builder.Services.AddSyncfusionBlazor();
await builder.Build().RunAsync();

// Server App
var builder = WebApplication.CreateBuilder(args);
builder.Services.AddRazorPages();
builder.Services.AddServerSideBlazor();
builder.Services.AddSyncfusionBlazor();
var app = builder.Build();

// Web App (register in both server and client Program.cs if using WebAssembly/Auto render mode)
builder.Services.AddSyncfusionBlazor();
```

## Setup for Blazor WebAssembly

### Add Stylesheet and Script References

In `~/App.razor` or `~/Components/App.razor`:

```html
<link href="_content/Syncfusion.Blazor.Themes/fluent2.css" rel="stylesheet" />
<!-- Other stylesheets -->

<script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js" type="text/javascript"></script>
```

### Theme Options

Available themes:
- `fluent2.css` - Fluent 2 Design
- `bootstrap5.css` - Bootstrap 5
- `tailwind.css` - Tailwind CSS
- `material.css` - Material Design
- `fluent.css` - Fluent Design

Choose one that matches your design system.

## Setup for Blazor Web App

In `~/Components/App.razor`, add references using Static Web Assets:

```html
<link href="_content/Syncfusion.Blazor.Themes/fluent2.css" rel="stylesheet" />
<script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js" type="text/javascript"></script>
```

**Render Mode Configuration:**
- If using `Per page/component` interactivity, add `@rendermode InteractiveAuto` (or `InteractiveServer`/`InteractiveWebAssembly`) at the top of your Razor file
- If using `Global` interactivity, render mode is configured automatically in `App.razor`

## Setup for Blazor Server App

In `~/_Layout.cshtml` or `~/Components/App.razor`:

```html
<link href="_content/Syncfusion.Blazor.Themes/fluent2.css" rel="stylesheet" />
<script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js" type="text/javascript"></script>
```

## Initial Component Setup

### Minimal ListView Example

```cshtml
@using Syncfusion.Blazor.Lists

<SfListView DataSource="@ListData">
    <ListViewFieldSettings TValue="DataModel" Id="Id" Text="Text"></ListViewFieldSettings>
</SfListView>

@code {
    List<DataModel> ListData = new List<DataModel>();

    protected override void OnInitialized()
    {
        ListData.Add(new DataModel { Text = "Item 1", Id = "1" });
        ListData.Add(new DataModel { Text = "Item 2", Id = "2" });
        ListData.Add(new DataModel { Text = "Item 3", Id = "3" });
    }

    public class DataModel
    {
        public string Id { get; set; }
        public string Text { get; set; }
    }
}
```

### ListView with Header

```cshtml
<SfListView DataSource="@ListData" 
            HeaderTitle="My List" 
            ShowHeader="true">
    <ListViewFieldSettings TValue="DataModel" Id="Id" Text="Text"></ListViewFieldSettings>
</SfListView>
```

## Complete Example

```cshtml
@using Syncfusion.Blazor.Lists

<div style="max-width: 400px; margin: 20px;">
    <SfListView DataSource="@ListData" 
                HeaderTitle="Cars" 
                ShowHeader="true"
                Width="100%">
        <ListViewFieldSettings TValue="CarModel" Id="Id" Text="Name"></ListViewFieldSettings>
    </SfListView>
</div>

@code {
    List<CarModel> ListData = new List<CarModel>();

    protected override void OnInitialized()
    {
        base.OnInitialized();
        
        ListData.Add(new CarModel { Name = "Hennessey Venom", Id = "1" });
        ListData.Add(new CarModel { Name = "Bugatti Chiron", Id = "2" });
        ListData.Add(new CarModel { Name = "Bugatti Veyron Super Sport", Id = "3" });
        ListData.Add(new CarModel { Name = "SSC Ultimate Aero", Id = "4" });
        ListData.Add(new CarModel { Name = "Koenigsegg CCR", Id = "5" });
    }

    public class CarModel
    {
        public string Id { get; set; }
        public string Name { get; set; }
    }
}
```

---

**Next Steps:**
- Bind data from sources in [Data Binding and Sources](data-binding-and-source.md)
- Customize appearance with [Templating and Rendering](templating-and-rendering.md)
- Enable selection in [Item Selection and Actions](item-selection-and-actions.md)

