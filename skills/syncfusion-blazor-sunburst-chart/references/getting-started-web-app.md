# Getting Started with Blazor Sunburst Chart — Blazor Web App

This guide covers the Blazor Web App template, which supports Server, WebAssembly, and Auto interactive render modes (the unified .NET 8/9/10 model).

## Table of Contents
- [Using .NET CLI templates (recommended)](#using-net-cli-templates-recommended)
- [Manually creating a new Blazor Web App](#manually-creating-a-new-blazor-web-app)
- [Install the required Blazor package](#install-the-required-blazor-package)
- [Add import namespaces](#add-import-namespaces)
- [Register the Blazor service](#register-the-blazor-service)
- [Add script resource](#add-script-resource)
- [Add the Blazor Sunburst Chart component](#add-the-blazor-sunburst-chart-component)
- [Run the application](#run-the-application)
- [See also](#see-also)

## Using .NET CLI templates (recommended)

Quickly set up a Syncfusion-preconfigured Blazor Web App:

```bash
dotnet new install Syncfusion.Blazor.WebApp.Templates
dotnet new syncfusionblazorwebapp --name MyApp --interactivity Server --all-interactive Global
cd MyApp
dotnet run
```

Replace `--interactivity Server` with `--interactivity WebAssembly` or `--interactivity Auto` for those render modes. For per-page/component interactivity, replace `--all-interactive Global` with `--all-interactive PerPage/component` (or omit it).

## Manually creating a new Blazor Web App

```bash
dotnet new blazor -o BlazorWebApp --interactivity Auto
cd BlazorWebApp
cd BlazorWebApp.Client
```

Configure the appropriate interactive render mode and interactivity location while creating the app.

## Install the required Blazor package

Install `Syncfusion.Blazor.Charts`. If using `WebAssembly` or `Auto` render modes, install this package in the `.Client` project.

```bash
dotnet add package Syncfusion.Blazor.Charts -v 35.x.x
```

Or via Visual Studio → Tools → NuGet Package Manager → Manage NuGet Packages for Solution, and search for `Syncfusion.Blazor.Charts`.

## Add import namespaces

Open `~/_Imports.razor` from the `.Client` project (or the main project for Server mode) and import the namespaces:

```cshtml
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Charts
```

## Register the Blazor service

Open `Program.cs` and register the service:

```csharp
using Syncfusion.Blazor;
// ...
builder.Services.AddSyncfusionBlazor();
```

If the interactive render mode is `WebAssembly` or `Auto`, register the Blazor service in the `Program.cs` files of **both** the server and client projects.

## Add script resource

The Sunburst Chart ships in the `Syncfusion.Blazor.Charts` package, which provides a single combined chart script used by every chart component (including the Sunburst Chart). Add the script reference at the end of the `<body>` in `App.razor`:

```cshtml
<script src="_content/Syncfusion.Blazor.Charts/scripts/sf-sunburst-chart.js" type="text/javascript"></script>
```

## Add the Blazor Sunburst Chart component

Open a Razor file in the `.Client` project, e.g., `~/Pages/Home.razor`:

```cshtml
@rendermode InteractiveAuto

<SfSunburstChart TItem="NodeDetails"
                 Title="Population by Region"
                 DataSource="@DataSource"
                 IdMemberPath="@nameof(NodeDetails.Id)"
                 ParentIdMemberPath="@nameof(NodeDetails.ParentId)"
                 ValueMemberPath="@nameof(NodeDetails.Value)"
                 LabelMemberPath="@nameof(NodeDetails.Name)"
                 Width="100%"
                 Height="450px">
</SfSunburstChart>

@code {
    public class NodeDetails
    {
        public string Id { get; set; } = string.Empty;
        public string? ParentId { get; set; }
        public string Name { get; set; } = string.Empty;
        public double Value { get; set; }
    }

    public List<NodeDetails> DataSource = new()
    {
        new NodeDetails { Id = "USA", ParentId = null, Name = "USA" },
        new NodeDetails { Id = "USA-West", ParentId = "USA", Name = "West", Value = 39 },
        new NodeDetails { Id = "USA-East", ParentId = "USA", Name = "East", Value = 47 },
        new NodeDetails { Id = "India", ParentId = null, Name = "India" },
        new NodeDetails { Id = "India-North", ParentId = "India", Name = "North", Value = 67 },
        new NodeDetails { Id = "India-South", ParentId = "India", Name = "South", Value = 41 },
        new NodeDetails { Id = "Germany", ParentId = null, Name = "Germany" },
        new NodeDetails { Id = "Germany-Bavaria", ParentId = "Germany", Name = "Bavaria", Value = 13 },
        new NodeDetails { Id = "Germany-Berlin", ParentId = "Germany", Name = "Berlin", Value = 4 }
    };
}
```

> For per-page/component interactivity, define a render mode at the top of the Razor file (`InteractiveServer`, `InteractiveWebAssembly`, or `InteractiveAuto`). For global `Auto` or `WebAssembly` interactivity, the render mode is configured automatically in `App.razor`.

## Run the application

```bash
dotnet run
```

Navigate to the page hosting the Sunburst Chart component. The chart renders in your default web browser.

## See also

- [Getting Started — Blazor Server App](getting-started.md)
- [Getting Started — Standalone WebAssembly](getting-started-wasm.md)
- [Working with Data](working-with-data.md)
