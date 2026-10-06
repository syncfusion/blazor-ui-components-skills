# Getting Started with Blazor Sunburst Chart — Standalone WebAssembly

This guide covers the standalone Blazor WebAssembly App template (not the unified Blazor Web App).

## Table of Contents
- [Using .NET CLI templates](#using-net-cli-templates)
- [Manually creating a standalone WebAssembly App](#manually-creating-a-standalone-webassembly-app)
- [Install the required Blazor package](#install-the-required-blazor-package)
- [Add import namespaces](#add-import-namespaces)
- [Register the Blazor service](#register-the-blazor-service)
- [Add script resource](#add-script-resource)
- [Add the Blazor Sunburst Chart component](#add-the-blazor-sunburst-chart-component)
- [Run the application](#run-the-application)
- [See also](#see-also)

## Using .NET CLI templates

```bash
dotnet new install Syncfusion.Blazor.WebAssemblyApp.Templates
dotnet new syncfusionblazorwasmapp --name MyApp --pwa true
cd MyApp
dotnet run
```

## Manually creating a standalone WebAssembly App

```bash
dotnet new blazorwasm -o BlazorApp
cd BlazorApp
```

## Install the required Blazor package

Install the `Syncfusion.Blazor.Charts` NuGet package:

```bash
dotnet add package Syncfusion.Blazor.Charts -v 35.x.x
```

Or via Visual Studio → Tools → NuGet Package Manager, and search for `Syncfusion.Blazor.Charts`.

## Add import namespaces

Open `~/_Imports.razor` and import the namespaces:

```cshtml
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Charts
```

## Register the Blazor service

Open `Program.cs` and add `using Syncfusion.Blazor;` at the top, then register the service:

```csharp
builder.Services.AddSyncfusionBlazor();
```

## Add script resource

The Sunburst Chart ships in the `Syncfusion.Blazor.Charts` package, which provides a single combined chart script used by every chart component (including the Sunburst Chart). Add the script reference at the end of the `<body>` in `~/wwwroot/index.html`:

```cshtml
<script src="_content/Syncfusion.Blazor.Charts/scripts/sf-sunburst-chart.js" type="text/javascript"></script>
```

## Add the Blazor Sunburst Chart component

Open a Razor file in `~/Pages/`, e.g., `Home.razor`:

```cshtml
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

## Run the application

```bash
dotnet run
```

Navigate to the page hosting the Sunburst Chart component. The chart renders in your default web browser.

## See also

- [Getting Started — Blazor Server App](getting-started.md)
- [Getting Started — Blazor Web App](getting-started-web-app.md)
- [Working with Data](working-with-data.md)
