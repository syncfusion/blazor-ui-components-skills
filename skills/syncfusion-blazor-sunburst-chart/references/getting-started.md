# Getting Started with Blazor Sunburst Chart — Blazor Server App

This guide walks through adding the `Blazor Sunburst Chart` component to a Blazor Server App using the .NET CLI. The same package also works in Blazor Web App and standalone WebAssembly; see the dedicated references for those hosting models.

## Table of Contents
- [Prerequisites](#prerequisites)
- [Install the NuGet package](#install-the-nuget-package)
- [Add import namespaces](#add-import-namespaces)
- [Register the Blazor service](#register-the-blazor-service)
- [Add script resources](#add-script-resources)
- [Add the Blazor Sunburst Chart component](#add-the-blazor-sunburst-chart-component)
- [Run the application](#run-the-application)
- [See also](#see-also)

## Prerequisites

- .NET 8/9/10 SDK
- A Blazor Server App created from the `blazor` template:

```bash
dotnet new blazor -o BlazorApp --interactivity Server
cd BlazorApp
```

Configure the appropriate interactive render mode and interactivity location while creating the app. For per-page/component interactivity, set the render mode at the top of the razor file.

## Install the NuGet package

Install the `Syncfusion.Blazor.Charts` NuGet package — it contains every Syncfusion Blazor chart component, including the Sunburst Chart.

```bash
dotnet add package Syncfusion.Blazor.Charts -v 35.x.x
```

Or via the Visual Studio Package Manager Console:

```
Install-Package Syncfusion.Blazor.Charts -Version 35.x.x
```

## Add import namespaces

Open `~/Components/_Imports.razor` and import the required namespaces:

```cshtml
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Charts
```

## Register the Blazor service

Open `Program.cs` and add `using Syncfusion.Blazor;` at the top, then register the service:

```csharp
builder.Services.AddSyncfusionBlazor();
```

## Add script resources

The script is accessible from NuGet through Static Web Assets. The Sunburst Chart ships in the `Syncfusion.Blazor.Charts` package, which provides a single combined chart script used by every chart component (including the Sunburst Chart). Add the script reference at the end of the `<body>` in `~/Components/App.razor`:

```cshtml
<script src="_content/Syncfusion.Blazor.Charts/scripts/sf-sunburst-chart.js" type="text/javascript"></script>
```

## Add the Blazor Sunburst Chart component

Open a Razor file such as `~/Components/Pages/Home.razor`:

```cshtml
@rendermode InteractiveServer

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

> If interactivity is set to `Per page/component`, define a render mode at the top of the Razor file (e.g., `@rendermode InteractiveServer`). If interactivity is `Global`, the render mode is configured automatically in `App.razor`.

## Run the application

```bash
dotnet run
```

Navigate to the page hosting the Sunburst Chart component. The chart renders in your default web browser.

## See also

- [Working with Data](working-with-data.md) for `DataSource` and member paths.
- [Appearance](appearance.md) for themes, palette, border, margin, and ring geometry.
- [Getting Started — Blazor Web App](getting-started-web-app.md) for the Web App hosting model.
