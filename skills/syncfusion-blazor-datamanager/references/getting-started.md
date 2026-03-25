# Getting Started with Blazor DataManager

This guide covers setting up `SfDataManager` in a new Blazor project and rendering your first data-bound component.

## Table of Contents
- [Prerequisites](#prerequisites)
- [Install NuGet Packages](#install-nuget-packages)
- [Configure the Application](#configure-the-application)
- [Add Stylesheet and Script Resources](#add-stylesheet-and-script-resources)
- [Binding to JSON Data](#binding-to-json-data)
- [Binding to OData (Remote)](#binding-to-odata-remote)
- [Component Binding with SfDropDownList](#component-binding-with-sfdropdownlist)

---

## Prerequisites

Ensure the development environment meets the system requirements for Syncfusion Blazor components before proceeding (.NET 9.0 or later recommended).

---

## Install NuGet Packages

Install the following packages in your Blazor project:

- `Syncfusion.Blazor.Data` — DataManager core
- `Syncfusion.Blazor.Themes` — Theme CSS files

**Visual Studio (Package Manager Console):**
```
Install-Package Syncfusion.Blazor.Data
Install-Package Syncfusion.Blazor.Themes
```

**dotnet CLI:**
```bash
dotnet add package Syncfusion.Blazor.Data
dotnet add package Syncfusion.Blazor.Themes
dotnet restore
```

> For **Blazor Web App** with WebAssembly or Auto render mode, install these packages in the **Client** project.

---

## Configure the Application

### Import Namespaces

Add to `~/_Imports.razor`:

```csharp
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Data
```

### Register Syncfusion Service

**Blazor WebAssembly — `~/Program.cs`:**

```csharp
using Syncfusion.Blazor;

var builder = WebAssemblyHostBuilder.CreateDefault(args);
builder.Services.AddSyncfusionBlazor();
await builder.Build().RunAsync();
```

**Blazor Web App (Auto or WebAssembly render mode) — Server `Program.cs`:**

```csharp
using Syncfusion.Blazor;

builder.Services.AddRazorComponents()
    .AddInteractiveServerComponents()
    .AddInteractiveWebAssemblyComponents();
builder.Services.AddSyncfusionBlazor();
```

**Blazor Web App (Auto or WebAssembly render mode) — Client `Program.cs`:**

```csharp
using Syncfusion.Blazor;

builder.Services.AddSyncfusionBlazor();
await builder.Build().RunAsync();
```

**Blazor Web App (Server render mode only) — `Program.cs`:**

```csharp
using Syncfusion.Blazor;

builder.Services.AddRazorComponents()
    .AddInteractiveServerComponents();
builder.Services.AddSyncfusionBlazor();
```

---

## Add Stylesheet and Script Resources

### Blazor WebAssembly — `~/wwwroot/index.html`

```html
<head>
    <link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
    <script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js" type="text/javascript"></script>
</head>
```

### Blazor Web App — `~/Components/App.razor`

```html
<!-- In <head> -->
<link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />

<!-- At end of <body> -->
<script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js" type="text/javascript"></script>
```

Available themes: `bootstrap5.css`, `material.css`, `fluent.css`, `tailwind.css`, `fabric.css`, `material-dark.css`

---

## Binding to JSON Data

Use the `Json` property of `SfDataManager` to bind an in-memory collection. The `SfDataManager` is placed as a child inside the data-bound component.

```razor
@using Syncfusion.Blazor.Data
@using Syncfusion.Blazor.Grids

<SfGrid TValue="EmployeeData" ID="Grid">
    <SfDataManager Json="@Employees"></SfDataManager>
    <GridColumns>
        <GridColumn Field="@nameof(EmployeeData.EmployeeID)" TextAlign="TextAlign.Center"
                    HeaderText="Employee ID" Width="120"></GridColumn>
        <GridColumn Field="@nameof(EmployeeData.Name)" HeaderText="First Name" Width="130"></GridColumn>
        <GridColumn Field="@nameof(EmployeeData.Title)" HeaderText="Title" Width="120"></GridColumn>
    </GridColumns>
</SfGrid>

@code {
    public class EmployeeData
    {
        public int EmployeeID { get; set; }
        public string Name { get; set; }
        public string Title { get; set; }
    }

    public List<EmployeeData> Employees = new()
    {
        new EmployeeData { EmployeeID = 1, Name = "Nancy Fuller", Title = "Vice President" },
        new EmployeeData { EmployeeID = 2, Name = "Steven Buchanan", Title = "Sales Manager" },
        new EmployeeData { EmployeeID = 3, Name = "Janet Leverling", Title = "Sales Representative" },
        new EmployeeData { EmployeeID = 4, Name = "Andrew Davolio", Title = "Inside Sales Coordinator" }
    };
}
```

---

## Binding to OData (Remote)

Set `Url` to the service endpoint and `Adaptor` to the matching adaptor type.

```razor
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Data
@using Syncfusion.Blazor.Grids

<SfGrid TValue="Order" ID="Grid" AllowPaging="true">
    <SfDataManager Url="https://services.odata.org/Northwind/Northwind.svc/Orders"
                   Adaptor="Adaptors.ODataAdaptor">
    </SfDataManager>
    <GridColumns>
        <GridColumn Field="@nameof(Order.OrderID)" HeaderText="Order ID" IsPrimaryKey="true"
                    TextAlign="TextAlign.Right" Width="120"></GridColumn>
        <GridColumn Field="@nameof(Order.CustomerID)" HeaderText="Customer Name" Width="150"></GridColumn>
        <GridColumn Field="@nameof(Order.OrderDate)" HeaderText="Order Date" Format="d"
                    Type="ColumnType.Date" TextAlign="TextAlign.Right" Width="130"></GridColumn>
        <GridColumn Field="@nameof(Order.Freight)" HeaderText="Freight" Format="C2"
                    TextAlign="TextAlign.Right" Width="120"></GridColumn>
    </GridColumns>
</SfGrid>

@code {
    public class Order
    {
        public int? OrderID { get; set; }
        public string? CustomerID { get; set; }
        public DateTime? OrderDate { get; set; }
        public double? Freight { get; set; }
    }
}
```

---

## Component Binding with SfDropDownList

`SfDataManager` works with any Syncfusion data-bound component, not just `SfGrid`.

### Local Data

```razor
@using Syncfusion.Blazor.Data
@using Syncfusion.Blazor.DropDowns

<SfDropDownList Placeholder="e.g. Australia" TItem="Country" TValue="string">
    <SfDataManager Json="@Countries"></SfDataManager>
    <DropDownListFieldSettings Value="Name"></DropDownListFieldSettings>
</SfDropDownList>

@code {
    public class Country
    {
        public string? Name { get; set; }
        public string? Code { get; set; }
    }

    public List<Country> Countries = new()
    {
        new Country { Name = "Australia", Code = "AU" },
        new Country { Name = "Bermuda", Code = "BM" },
        new Country { Name = "Canada", Code = "CA" },
        new Country { Name = "Cameroon", Code = "CM" }
    };
}
```

### Remote Data

```razor
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Data
@using Syncfusion.Blazor.DropDowns

<SfDropDownList Placeholder="Name" TItem="Contact" TValue="Contact">
    <SfDataManager Url="https://services.odata.org/V4/Northwind/Northwind.svc/Customers"
                   Adaptor="Adaptors.ODataV4Adaptor">
    </SfDataManager>
    <DropDownListFieldSettings Value="CustomerID" Text="ContactName"></DropDownListFieldSettings>
</SfDropDownList>

@code {
    public class Contact
    {
        public string? ContactName { get; set; }
        public string? CustomerID { get; set; }
    }
}
```
