# Getting Started with Blazor Dropdown Tree

## Table of Contents
- [NuGet Installation](#nuget-installation)
- [Namespace Setup](#namespace-setup)
- [Service Registration](#service-registration)
- [Theme Setup](#theme-setup)
- [Basic Implementation](#basic-implementation)
- [Setup for Web App](#setup-for-web-app)
- [Setup for Server App](#setup-for-server-app)

## NuGet Installation

The Dropdown Tree component requires the `Syncfusion.Blazor.Navigations` and `Syncfusion.Blazor.Themes` NuGet packages.

### Via Package Manager Console

```csharp
Install-Package Syncfusion.Blazor.Navigations -Version {{ site.releaseversion }}
Install-Package Syncfusion.Blazor.Themes -Version {{ site.releaseversion }}
```

### Via .NET CLI

```bash
dotnet add package Syncfusion.Blazor.Navigations -v {{ site.releaseversion }}
dotnet add package Syncfusion.Blazor.Themes -v {{ site.releaseversion }}
dotnet restore
```

## Namespace Setup

Add the following namespaces to your **~/_Imports.razor** file:

```razor
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Navigations
```

## Service Registration

Register the Syncfusion Blazor service in your **~/Program.cs**:

```csharp
using Microsoft.AspNetCore.Components.Web;
using Microsoft.AspNetCore.Components.WebAssembly.Hosting;
using Syncfusion.Blazor;

var builder = WebAssemblyHostBuilder.CreateDefault(args);
builder.RootComponents.Add<App>("#app");
builder.RootComponents.Add<HeadOutlet>("head::after");

builder.Services.AddScoped(sp => new HttpClient { BaseAddress = new Uri(builder.HostEnvironment.BaseAddress) });

// Add Syncfusion Blazor services
builder.Services.AddSyncfusionBlazor();
await builder.Build().RunAsync();
```

## Theme Setup

Add the theme stylesheet and script reference to your **~/index.html** file (inside the `<head>` section):

```html
<head>
    ...
    <!-- Syncfusion Blazor Bootstrap5 Theme -->
    <link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
    
    <!-- Syncfusion Blazor Core Script -->
    <script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js" type="text/javascript"></script>
</head>
```

**Available Themes:**
- `bootstrap5.css` - Bootstrap 5 Theme
- `material3.css` - Material Design 3 Theme
- `fluent2.css` - Microsoft Fluent 2 Theme
- `tailwind.css` - Tailwind CSS Theme

## Basic Implementation

Here's a minimal working example of the Dropdown Tree component with self-referential data:

```razor
@using Syncfusion.Blazor.Navigations

<SfDropDownTree TItem="EmployeeData" TValue="string" Placeholder="Select an employee" Width="500px">
    <DropDownTreeField TItem="EmployeeData" 
        DataSource="Data" 
        ID="Id" 
        Text="Name" 
        HasChildren="HasChild" 
        ParentID="PId">
    </DropDownTreeField>
</SfDropDownTree>

@code {
    List<EmployeeData> Data = new List<EmployeeData>
    {
        new EmployeeData() { Id = "1", Name = "Steven Buchanan", Job = "General Manager", HasChild = true, Expanded = true },
        new EmployeeData() { Id = "2", PId = "1", Name = "Laura Callahan", Job = "Product Manager", HasChild = true },
        new EmployeeData() { Id = "3", PId = "2", Name = "Andrew Fuller", Job = "Team Lead", HasChild = true },
        new EmployeeData() { Id = "4", PId = "3", Name = "Anne Dodsworth", Job = "Developer" },
        new EmployeeData() { Id = "10", PId = "3", Name = "Lilly", Job = "Developer" },
        new EmployeeData() { Id = "5", PId = "1", Name = "Nancy Davolio", Job = "Product Manager", HasChild = true },
        new EmployeeData() { Id = "6", PId = "5", Name = "Michael Suyama", Job = "Team Lead", HasChild = true },
        new EmployeeData() { Id = "7", PId = "6", Name = "Robert King", Job = "Developer" },
        new EmployeeData() { Id = "11", PId = "6", Name = "Mary", Job = "Developer" },
        new EmployeeData() { Id = "9", PId = "1", Name = "Janet Leverling", Job = "HR"}
    };

    public class EmployeeData
    {
        public string? Id { get; set; }
        public string? Name { get; set; }
        public string? Job { get; set; }
        public bool HasChild { get; set; }
        public bool Expanded { get; set; }
        public string? PId { get; set; }
    }
}
```

## DropDownTreeField Properties

The `DropDownTreeField` component is configured with field mappings:

| Property | Type | Description |
|----------|------|-------------|
| `ID` | string | Maps the unique identifier field of the data item |
| `Text` | string | Maps the text/display field of the data item |
| `ParentID` | string | Maps the parent identifier for self-referential data |
| `HasChildren` | string | Maps a boolean field indicating if node has children |
| `Child` | string | Maps nested child collection (for hierarchical data) |
| `DataSource` | IEnumerable | Specifies the local data source |
| `Expanded` | string | Maps a boolean field for default expand state |

## Setup for Web App (WebAssembly)

1. Create a new Blazor WebAssembly App using Visual Studio or CLI:
```bash
dotnet new blazorwasm -o BlazorApp
cd BlazorApp
```

2. Install NuGet packages:
```bash
dotnet add package Syncfusion.Blazor.Navigations -v {{ site.releaseversion }}
dotnet add package Syncfusion.Blazor.Themes -v {{ site.releaseversion }}
```

3. Update `_Imports.razor`:
```razor
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Navigations
```

4. Register Syncfusion service in `Program.cs`:
```csharp
builder.Services.AddSyncfusionBlazor();
```

5. Add theme and script to `index.html`

6. Use the component in your pages

## Setup for Server App

1. Create a new Blazor Server App using Visual Studio or CLI:
```bash
dotnet new blazorserver -o BlazorServerApp
cd BlazorServerApp
```

2. Install NuGet packages:
```bash
dotnet add package Syncfusion.Blazor.Navigations -v {{ site.releaseversion }}
dotnet add package Syncfusion.Blazor.Themes -v {{ site.releaseversion }}
```

3. Update `_Imports.razor`:
```razor
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Navigations
```

4. Register Syncfusion service in `Program.cs`:
```csharp
builder.Services.AddSyncfusionBlazor();
```

5. Add theme and script to `_Layout.cshtml` (in the `<head>` section):
```html
<link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
<script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js" type="text/javascript"></script>
```

6. Use the component in your pages

## Data Binding Modes

### Self-Referential Data

Data with ParentID relationship:

```csharp
List<TreeItem> data = new List<TreeItem>
{
    new TreeItem { Id = "1", Name = "Parent", ParentId = null },
    new TreeItem { Id = "2", Name = "Child 1", ParentId = "1" },
    new TreeItem { Id = "3", Name = "Child 2", ParentId = "1" }
};
```

### Hierarchical Data

Nested collection of objects:

```csharp
List<TreeItem> data = new List<TreeItem>
{
    new TreeItem 
    { 
        Id = "1", 
        Name = "Parent",
        Children = new List<TreeItem>
        {
            new TreeItem { Id = "2", Name = "Child 1" },
            new TreeItem { Id = "3", Name = "Child 2" }
        }
    }
};
```

Configure with:
```razor
<DropDownTreeField TItem="TreeItem" 
    DataSource="data" 
    ID="Id" 
    Text="Name" 
    Child="Children">
</DropDownTreeField>
```

## Rendering

The Dropdown Tree component renders as:
- **Input field**: Displays selected item(s)
- **Toggle button**: Opens/closes the tree popup
- **Popup container**: Contains the tree structure with nodes and hierarchy

Each tree node shows:
- Expand/collapse icon (if has children)
- Node text
- Checkbox (if ShowCheckBox is true)
