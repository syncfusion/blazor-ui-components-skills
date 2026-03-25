# Getting Started — Syncfusion Blazor Pivot Table

## Table of Contents
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Register Service & Add Resources](#register-service--add-resources)
- [Initialize the Component](#initialize-the-component)
- [Assign Data and Configure Axes](#assign-data-and-configure-axes)
- [Server App Setup](#server-app-setup)
- [MAUI Blazor Setup](#maui-blazor-setup)
- [Minimal Working Example](#minimal-working-example)

---

## Prerequisites

- .NET 6 / 7 / 8 / 9 / 10 SDK
- Visual Studio 2022+, VS Code, or .NET CLI
- A Blazor WebAssembly App, Blazor Server App, Blazor Web App, or MAUI Blazor project

---

## Installation

Install the NuGet packages:

**Visual Studio — Package Manager Console:**
```bash
Install-Package Syncfusion.Blazor.PivotTable
Install-Package Syncfusion.Blazor.Themes
```

**dotnet CLI:**
```bash
dotnet add package Syncfusion.Blazor.PivotTable
dotnet add package Syncfusion.Blazor.Themes
dotnet restore
```

---

## Register Service & Add Resources

### `_Imports.razor`
```razor
@using Syncfusion.Blazor
@using Syncfusion.Blazor.PivotView
```

### `Program.cs`
```csharp
using Syncfusion.Blazor;

// Blazor WASM
builder.Services.AddSyncfusionBlazor();
await builder.Build().RunAsync();

// Blazor Server (.NET 6/7)
builder.Services.AddSyncfusionBlazor();
```

### `wwwroot/index.html` (WASM) or `App.razor` / `_Layout.cshtml` (Server)
```html
<head>
    <link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
    <script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js"
            type="text/javascript"></script>
</head>
```

> **Available themes:** `bootstrap5.css`, `material.css`, `fluent.css`, `tailwind.css`, `fabric.css`

> **Optional Pivot Table script** (for advanced features like OLAP or WebAssembly performance):
> ```html
> <script src="_content/Syncfusion.Blazor.PivotTable/scripts/sf-pivotview.min.js"
>         type="text/javascript"></script>
> ```

---

## Initialize the Component

Add `SfPivotView` to a `.razor` page:

```razor
@page "/"
@using Syncfusion.Blazor.PivotView

<SfPivotView TValue="ProductDetails" Height="350"></SfPivotView>
```

`TValue` is the generic type parameter matching your data model class.

---

## Assign Data and Configure Axes

The four axes in `PivotViewDataSourceSettings`:
- **Columns** — field members displayed as column headers
- **Rows** — field members displayed as row headers
- **Values** — numeric fields to aggregate in cells
- **Filters** — fields available for filtering without display on grid

```razor
@page "/"
@using Syncfusion.Blazor.PivotView

<SfPivotView TValue="ProductDetails" Height="350">
    <PivotViewDataSourceSettings DataSource="@data" EnableSorting="true">
        <PivotViewColumns>
            <PivotViewColumn Name="Year"></PivotViewColumn>
            <PivotViewColumn Name="Quarter"></PivotViewColumn>
        </PivotViewColumns>
        <PivotViewRows>
            <PivotViewRow Name="Country"></PivotViewRow>
            <PivotViewRow Name="Products"></PivotViewRow>
        </PivotViewRows>
        <PivotViewValues>
            <PivotViewValue Name="Sold" Caption="Units Sold"></PivotViewValue>
            <PivotViewValue Name="Amount" Caption="Sold Amount"></PivotViewValue>
        </PivotViewValues>
        <PivotViewFormatSettings>
            <PivotViewFormatSetting Name="Amount" Format="C0" UseGrouping="true">
            </PivotViewFormatSetting>
        </PivotViewFormatSettings>
    </PivotViewDataSourceSettings>
</SfPivotView>

@code {
    public List<ProductDetails> data { get; set; }

    protected override void OnInitialized()
    {
        data = ProductDetails.GetProductData().ToList();
    }

    public class ProductDetails
    {
        public int Sold { get; set; }
        public double Amount { get; set; }
        public string Country { get; set; }
        public string Products { get; set; }
        public string Year { get; set; }
        public string Quarter { get; set; }

        public static List<ProductDetails> GetProductData()
        {
            return new List<ProductDetails>
            {
                new ProductDetails { Sold=31, Amount=52824, Country="France", Products="Mountain Bikes", Year="FY 2022", Quarter="Q1" },
                new ProductDetails { Sold=51, Amount=86904, Country="France", Products="Mountain Bikes", Year="FY 2022", Quarter="Q2" },
                new ProductDetails { Sold=90, Amount=153360, Country="France", Products="Mountain Bikes", Year="FY 2022", Quarter="Q3" },
                new ProductDetails { Sold=83, Amount=124422, Country="France", Products="Road Bikes", Year="FY 2022", Quarter="Q1" },
                new ProductDetails { Sold=51, Amount=92824, Country="Germany", Products="Mountain Bikes", Year="FY 2022", Quarter="Q1" },
                new ProductDetails { Sold=23, Amount=24422, Country="Germany", Products="Road Bikes", Year="FY 2022", Quarter="Q1" },
                new ProductDetails { Sold=91, Amount=67824, Country="United States", Products="Mountain Bikes", Year="FY 2022", Quarter="Q1" },
                new ProductDetails { Sold=53, Amount=94422, Country="United States", Products="Road Bikes", Year="FY 2022", Quarter="Q1" },
            };
        }
    }
}
```

---

## Server App Setup

For **Blazor Server** apps, the setup is identical to WASM except:
- Resources go in `~/Pages/_Layout.cshtml` (.NET 6/7) or `~/Components/App.razor` (.NET 8+)
- Use `builder.Services.AddSyncfusionBlazor()` in `Program.cs`

For **Blazor Web App** (.NET 8+), set the render mode for interactivity:
```razor
@rendermode InteractiveServer
@* or *@
@rendermode InteractiveWebAssembly
```

---

## MAUI Blazor Setup

For **MAUI Blazor** hybrid apps:

1. Install `Syncfusion.Blazor.PivotTable` in the MAUI project
2. Register in `MauiProgram.cs`:
   ```csharp
   builder.Services.AddSyncfusionBlazor();
   ```
3. Add CSS in `wwwroot/index.html`:
   ```html
   <link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
   ```
4. Use `SfPivotView` in any `.razor` component as normal

---

## Minimal Working Example

The smallest possible Pivot Table with data:

```razor
@using Syncfusion.Blazor.PivotView

<SfPivotView TValue="SalesData" Height="300">
    <PivotViewDataSourceSettings DataSource="@data">
        <PivotViewColumns>
            <PivotViewColumn Name="Year"></PivotViewColumn>
        </PivotViewColumns>
        <PivotViewRows>
            <PivotViewRow Name="Region"></PivotViewRow>
        </PivotViewRows>
        <PivotViewValues>
            <PivotViewValue Name="Revenue"></PivotViewValue>
        </PivotViewValues>
    </PivotViewDataSourceSettings>
</SfPivotView>

@code {
    record SalesData(string Region, string Year, double Revenue);

    List<SalesData> data = new()
    {
        new("North", "FY 2023", 10000),
        new("North", "FY 2024", 12000),
        new("South", "FY 2023", 8000),
        new("South", "FY 2024", 9500),
    };
}
```
