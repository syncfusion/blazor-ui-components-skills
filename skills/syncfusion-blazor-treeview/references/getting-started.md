# Getting Started with Blazor TreeView

This reference covers installation, project setup, and creating your first TreeView component in Blazor applications.

## Table of Contents

1. [Installation](#installation)
2. [Project Setup by Application Type](#project-setup-by-application-type)
3. [CSS Theme Configuration](#css-theme-configuration)
4. [Service Registration](#service-registration)
5. [Creating Your First TreeView](#creating-your-first-treeview)
6. [Data Binding](#data-binding)
7. [Validation Checklist](#validation-checklist)
8. [Common Issues & Troubleshooting](#common-issues--troubleshooting)

---

## Installation

### NuGet Package Installation

The Blazor TreeView component is part of the Syncfusion Navigations package. Install the required NuGet packages:

```
Install-Package Syncfusion.Blazor.Navigations -Version 26.1.35
Install-Package Syncfusion.Blazor.Themes -Version 26.1.35
```

Or using .NET CLI:

```bash
dotnet add package Syncfusion.Blazor.Navigations --version 26.1.35
dotnet add package Syncfusion.Blazor.Themes --version 26.1.35
dotnet restore
```

## Project Setup by Application Type

### Blazor WebAssembly App

1. **Create project** using Visual Studio or .NET CLI:
   ```bash
   dotnet new blazorwasm -o BlazorApp
   cd BlazorApp
   ```

2. **Install NuGet packages** (see above)

3. **Update ~/_Imports.razor**:
   ```csharp
   @using Syncfusion.Blazor
   @using Syncfusion.Blazor.Navigations
   ```

4. **Register service in Program.cs**:
   ```csharp
   builder.Services.AddSyncfusionBlazor();
   ```

5. **Add theme in Index.html** (head section):
   ```html
   <link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
   ```

### Blazor Server App

1. **Create project**:
   ```bash
   dotnet new blazorserver -o BlazorApp
   cd BlazorApp
   ```

2. **Install NuGet packages** (see above)

3. **Update _Imports.razor**:
   ```csharp
   @using Syncfusion.Blazor
   @using Syncfusion.Blazor.Navigations
   ```

4. **Register service in Program.cs**:
   ```csharp
   builder.Services.AddSyncfusionBlazor();
   ```

5. **Add theme in _Layout.cshtml** (head section):
   ```html
   <link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
   ```

### Blazor Web App (Interactive)

1. **Create project**:
   ```bash
   dotnet new blazor -o BlazorApp -int Auto
   cd BlazorApp
   ```

2. **Install NuGet packages in Client project** (.csproj)

3. **Update Client project _Imports.razor**:
   ```csharp
   @using Syncfusion.Blazor
   @using Syncfusion.Blazor.Navigations
   ```

4. **Register service in Program.cs** (Client project):
   ```csharp
   builder.Services.AddSyncfusionBlazor();
   ```

5. **Add theme in Client project Index.html**:
   ```html
   <link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
   ```

### MAUI Blazor App

1. **Create project**:
   ```bash
   dotnet new maui -n MauiApp
   cd MauiApp
   ```

2. **Install NuGet packages** in the MAUI project

3. **Update _Imports.razor**:
   ```csharp
   @using Syncfusion.Blazor
   @using Syncfusion.Blazor.Navigations
   ```

4. **Register service in MauiProgram.cs**:
   ```csharp
   .AddMauiBlazorWebView()
   .ConfigureSyncfusionBlazor();
   ```

5. **Add theme in wwwroot/index.html**:
   ```html
   <link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
   ```

## CSS Theme Configuration

Syncfusion provides multiple built-in themes:

```html
<!-- Bootstrap 5 Theme -->
<link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />

<!-- Material Theme -->
<link href="_content/Syncfusion.Blazor.Themes/material.css" rel="stylesheet" />

<!-- Fluent Theme -->
<link href="_content/Syncfusion.Blazor.Themes/fluent.css" rel="stylesheet" />

<!-- Tailwind Theme -->
<link href="_content/Syncfusion.Blazor.Themes/tailwind.css" rel="stylesheet" />
```

## First TreeView Component

### Minimal Example

```csharp
@using Syncfusion.Blazor.Navigations

<SfTreeView TValue="TreeData">
    <TreeViewFieldsSettings TValue="TreeData" 
        Id="Id" 
        Text="Name" 
        Child="Children" 
        DataSource="@TreeItems">
    </TreeViewFieldsSettings>
</SfTreeView>

@code {
    public class TreeData
    {
        public string? Id { get; set; }
        public string? Name { get; set; }
        public List<TreeData>? Children { get; set; }
    }

    List<TreeData> TreeItems = new()
    {
        new TreeData { Id = "1", Name = "Documents" },
        new TreeData { Id = "2", Name = "Images" },
        new TreeData { Id = "3", Name = "Music" }
    };
}
```

### Example with Hierarchical Data

```csharp
@using Syncfusion.Blazor.Navigations

<SfTreeView TValue="Folder">
    <TreeViewFieldsSettings TValue="Folder" 
        Id="Id" 
        Text="FolderName" 
        Child="SubFolders" 
        Expanded="Expanded"
        DataSource="@FolderData">
    </TreeViewFieldsSettings>
</SfTreeView>

@code {
    public class Folder
    {
        public string? Id { get; set; }
        public string? FolderName { get; set; }
        public bool Expanded { get; set; }
        public List<Folder>? SubFolders { get; set; }
    }

    List<Folder> FolderData = new();

    protected override void OnInitialized()
    {
        var documents = new List<Folder>
        {
            new Folder { Id = "1-1", FolderName = "My Documents" },
            new Folder { Id = "1-2", FolderName = "Downloads" }
        };

        FolderData.Add(new Folder
        {
            Id = "1",
            FolderName = "Documents",
            Expanded = true,
            SubFolders = documents
        });
    }
}
```

## Service Registration

The `AddSyncfusionBlazor()` method registers all Syncfusion Blazor components globally. Call it in Program.cs:

```csharp
using Syncfusion.Blazor;

var builder = WebAssemblyHostBuilder.CreateDefault(args);
builder.RootComponents.Add<App>("#app");

// Register Syncfusion services
builder.Services.AddSyncfusionBlazor();

await builder.Build().RunAsync();
```

## Validation Checklist

- [ ] NuGet packages installed (Navigations + Themes)
- [ ] Namespaces added to _Imports.razor
- [ ] Service registered in Program.cs
- [ ] CSS theme link added to HTML
- [ ] TreeView component renders in browser
- [ ] Data displays correctly

## Common Issues & Troubleshooting

**Issue: "SfTreeView type not found"**
- Ensure `@using Syncfusion.Blazor.Navigations` is in _Imports.razor

**Issue: Styles not applying**
- Verify CSS theme link is correct in HTML head
- Check browser console for 404 errors on CSS files

**Issue: Data not displaying**
- Verify DataSource is populated
- Check field mappings (Id, Text, Child) match data class properties
- Ensure data is initialized in OnInitialized()

**Issue: Component not rendering**
- Ensure Syncfusion service is registered in Program.cs
- Check for JavaScript console errors
- Verify project target framework is .NET 6.0 or later

## Next Steps

- Proceed to [data-binding.md](data-binding.md) to learn different data binding approaches
- Use [node-selection.md](node-selection.md) to enable user interaction
- Check [events-handling.md](events-handling.md) for handling TreeView events
