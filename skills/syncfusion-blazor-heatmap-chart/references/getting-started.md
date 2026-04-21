# Getting Started with Blazor HeatMap Chart

This guide explains how to set up and implement the Syncfusion Blazor HeatMap Chart component in a Blazor WebAssembly application using Visual Studio, Visual Studio Code, or the .NET CLI.

## Table of Contents

- [Prerequisites](#prerequisites)
- [Creating a New Blazor WebAssembly App](#creating-a-new-blazor-webassembly-app)
  - [Using Visual Studio](#using-visual-studio)
  - [Using Visual Studio Code](#using-visual-studio-code)
  - [Using .NET CLI](#using-net-cli)
- [Installing NuGet Packages](#installing-nuget-packages)
  - [Visual Studio Installation](#visual-studio-installation)
  - [Visual Studio Code Installation](#visual-studio-code-installation)
  - [.NET CLI Installation](#net-cli-installation)
- [Adding Import Namespaces](#adding-import-namespaces)
- [Registering Syncfusion Blazor Service](#registering-syncfusion-blazor-service)
- [Adding Stylesheet and Script Resources](#adding-stylesheet-and-script-resources)
- [Adding the HeatMap Component](#adding-the-heatmap-component)
- [Running the Application](#running-the-application)
- [Troubleshooting](#troubleshooting)

## Prerequisites

Before getting started, ensure your system meets the requirements for Blazor components:

- **.NET SDK**: .NET 6.0 or later installed on your machine
- **Development Environment**: Visual Studio 2022, Visual Studio Code, or any text editor with .NET CLI
- **Web Browser**: Modern web browser (Chrome, Firefox, Edge, Safari)
- **Syncfusion License**: Valid Syncfusion license or trial license (register for free trial at syncfusion.com)

To check your .NET SDK version, run:
```bash
dotnet --version
```

## Creating a New Blazor WebAssembly App

### Using Visual Studio

1. Open **Visual Studio 2022** (or later)
2. Click **Create a new project**
3. Search for **Blazor WebAssembly App** template
4. Select the template and click **Next**
5. Configure your project:
   - **Project name**: Enter a name (e.g., `BlazorHeatMapApp`)
   - **Location**: Choose a folder location
   - **Solution name**: Keep default or customize
6. Click **Next**
7. Select the framework version (.NET 6.0 or later)
8. Click **Create**

The project structure will be created with necessary files including `Program.cs`, `index.html`, and `_Imports.razor`.

### Using Visual Studio Code

1. Open **Visual Studio Code**
2. Press `Ctrl + `` (backtick) to open the integrated terminal
3. Navigate to your desired folder location
4. Run the following commands:

```bash
dotnet new blazorwasm -o BlazorHeatMapApp
cd BlazorHeatMapApp
```

This creates a new Blazor WebAssembly application in the `BlazorHeatMapApp` folder.

5. Open the folder in VS Code:
```bash
code .
```

### Using .NET CLI

Open a command prompt (Windows), terminal (macOS), or command shell (Linux) and run:

```bash
dotnet new blazorwasm -o BlazorHeatMapApp
cd BlazorHeatMapApp
```

This creates a new Blazor WebAssembly project with all necessary files and folder structure.

## Installing NuGet Packages

The Syncfusion Blazor HeatMap Chart requires two NuGet packages:
- **Syncfusion.Blazor.HeatMap**: HeatMap component package
- **Syncfusion.Blazor.Themes**: Theme stylesheets package

### Visual Studio Installation

1. Open your Blazor project in Visual Studio
2. Go to **Tools → NuGet Package Manager → Manage NuGet Packages for Solution**
3. Click the **Browse** tab
4. Search for `Syncfusion.Blazor.HeatMap`
5. Select the package and click **Install**
6. Accept the license terms
7. Repeat steps 4-6 for `Syncfusion.Blazor.Themes`

**Alternative: Package Manager Console**

Open Package Manager Console (**Tools → NuGet Package Manager → Package Manager Console**) and run:

```powershell
Install-Package Syncfusion.Blazor.HeatMap
Install-Package Syncfusion.Blazor.Themes
```

### Visual Studio Code Installation

1. Open the integrated terminal in VS Code (`Ctrl + `` )
2. Ensure you're in the project root directory (where the `.csproj` file is located)
3. Run the following commands:

```bash
dotnet add package Syncfusion.Blazor.HeatMap
dotnet add package Syncfusion.Blazor.Themes
dotnet restore
```

The `dotnet restore` command ensures all dependencies are properly installed.

### .NET CLI Installation

Open a command prompt or terminal, navigate to your project folder, and run:

```bash
dotnet add package Syncfusion.Blazor.HeatMap
dotnet add package Syncfusion.Blazor.Themes
dotnet restore
```

**Verify Installation**: Check your `.csproj` file. It should contain:

```xml
<ItemGroup>
  <PackageReference Include="Syncfusion.Blazor.HeatMap" Version="xx.x.x.x" />
  <PackageReference Include="Syncfusion.Blazor.Themes" Version="xx.x.x.x" />
</ItemGroup>
```

## Adding Import Namespaces

To use Syncfusion components throughout your application, add the required namespaces to the `_Imports.razor` file:

1. Open **~/_Imports.razor** file in your project
2. Add the following lines at the end:

```razor
@using Syncfusion.Blazor
@using Syncfusion.Blazor.HeatMap
```

**Complete _Imports.razor example:**

```razor
@using System.Net.Http
@using System.Net.Http.Json
@using Microsoft.AspNetCore.Components.Forms
@using Microsoft.AspNetCore.Components.Routing
@using Microsoft.AspNetCore.Components.Web
@using Microsoft.AspNetCore.Components.Web.Virtualization
@using Microsoft.AspNetCore.Components.WebAssembly.Http
@using Microsoft.JSInterop
@using BlazorHeatMapApp
@using BlazorHeatMapApp.Shared
@using Syncfusion.Blazor
@using Syncfusion.Blazor.HeatMap
```

This makes Syncfusion components available across all Razor components without needing to add using statements in each file.

## Registering Syncfusion Blazor Service

Register the Syncfusion Blazor service in the **Program.cs** file to enable component functionality:

1. Open **~/Program.cs** file
2. Add the Syncfusion.Blazor namespace:
```csharp
using Syncfusion.Blazor;
```

3. Register the service by adding `AddSyncfusionBlazor()` before `await builder.Build().RunAsync();`:

```csharp
using Microsoft.AspNetCore.Components.Web;
using Microsoft.AspNetCore.Components.WebAssembly.Hosting;
using Syncfusion.Blazor;

var builder = WebAssemblyHostBuilder.CreateDefault(args);
builder.RootComponents.Add<App>("#app");
builder.RootComponents.Add<HeadOutlet>("head::after");

builder.Services.AddScoped(sp => new HttpClient { BaseAddress = new Uri(builder.HostEnvironment.BaseAddress) });

// Register Syncfusion Blazor service
builder.Services.AddSyncfusionBlazor();

await builder.Build().RunAsync();
```

**Important**: The `AddSyncfusionBlazor()` call must be made before building and running the application.

## Adding Stylesheet and Script Resources

Syncfusion components require theme stylesheets and JavaScript files to function properly.

### Add References to index.html

1. Open **~/wwwroot/index.html** (for Blazor WebAssembly)
2. Add the stylesheet and script references inside the `<head>` tag:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no" />
    <title>BlazorHeatMapApp</title>
    <base href="/" />
    <link href="css/bootstrap/bootstrap.min.css" rel="stylesheet" />
    <link href="css/app.css" rel="stylesheet" />
    <link href="BlazorHeatMapApp.styles.css" rel="stylesheet" />
    
    <!-- Syncfusion Blazor Theme -->
    <link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
    
    <!-- Syncfusion Blazor Script -->
    <script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js" type="text/javascript"></script>
</head>
<body>
    <div id="app">Loading...</div>
    <!-- ... rest of body content ... -->
</body>
</html>
```

### Available Themes

Syncfusion provides multiple built-in themes. Replace `bootstrap5.css` with any of these:

- `bootstrap5.css` - Bootstrap 5 theme
- `material.css` - Material theme
- `fabric.css` - Fabric theme
- `fluent.css` - Fluent theme
- `tailwind.css` - Tailwind CSS theme
- `bootstrap4.css` - Bootstrap 4 theme
- `bootstrap.css` - Bootstrap 3 theme
- `material-dark.css` - Material dark theme
- `fabric-dark.css` - Fabric dark theme
- `fluent-dark.css` - Fluent dark theme
- `tailwind-dark.css` - Tailwind dark theme

**Example using Material theme:**
```html
<link href="_content/Syncfusion.Blazor.Themes/material.css" rel="stylesheet" />
```

## Adding the HeatMap Component

Now add the HeatMap Chart component to a Razor page:

1. Open **~/Pages/Index.razor** (or create a new Razor component)
2. Add the following code:

```razor
@page "/"
@using Syncfusion.Blazor.HeatMap

<h3>Blazor HeatMap Chart - Sales Revenue</h3>

<SfHeatMap DataSource="@HeatMapData">
    <HeatMapTitleSettings Text="Sales Revenue per Employee (in 1000 US$)"></HeatMapTitleSettings>
    <HeatMapCellSettings ShowLabel="true" TileType="CellType.Rect"></HeatMapCellSettings>
</SfHeatMap>

@code {
    int[,] GetDefaultData()
    {
        int[,] dataSource = new int[,]
        {
            {52, 65, 67, 45, 37, 52},
            {68, 52, 63, 51, 30, 51},
            {7, 16, 47, 47, 88, 6},
            {66, 64, 46, 40, 47, 41},
            {14, 46, 97, 69, 69, 3},
            {54, 46, 61, 46, 40, 39}
        };
        return dataSource;
    }
    
    public object HeatMapData { get; set; }
    
    protected override void OnInitialized()
    {
        HeatMapData = GetDefaultData();
    }
}
```

**Code Explanation:**

- `@page "/"`: Defines the route for this component
- `SfHeatMap`: The main HeatMap component
- `DataSource="@HeatMapData"`: Binds the data array to the component
- `HeatMapTitleSettings`: Configures the chart title
- `HeatMapCellSettings`: Configures cell display (labels, tile type)
- `GetDefaultData()`: Creates a 2D integer array with sample data (6x6 matrix)
- `OnInitialized()`: Lifecycle method that initializes data when component loads

## Running the Application

### Visual Studio
- Press **F5** or **Ctrl + F5** to run the application
- The application will launch in your default browser

### Visual Studio Code
- Press **F5** to start debugging
- Or run from terminal:
```bash
dotnet run
```
- Open browser and navigate to the URL shown in terminal (typically `https://localhost:5001`)

### .NET CLI
Run from terminal:
```bash
dotnet run
```
- Open your browser and go to the displayed URL

**Expected Result**: You should see a HeatMap Chart displaying a 6x6 grid with color-coded cells representing the sales revenue values. Each cell shows the numeric value, and colors indicate the magnitude of values.

## Troubleshooting

### Issue: HeatMap Not Displaying

**Solution:**
- Verify NuGet packages are installed correctly (check `.csproj` file)
- Ensure `AddSyncfusionBlazor()` is called in `Program.cs`
- Check that stylesheet and script references are in `index.html`
- Verify namespace imports in `_Imports.razor`

### Issue: "The type or namespace name 'Syncfusion' could not be found"

**Solution:**
- Run `dotnet restore` to restore NuGet packages
- Clean and rebuild the solution
- Verify package references in `.csproj` file

### Issue: License Error or Watermark Displayed

**Solution:**
- Register for a free Syncfusion trial license
- Add license key in `Program.cs` before `AddSyncfusionBlazor()`:
```csharp
Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense("YOUR_LICENSE_KEY");
```

### Issue: Styles Not Applied

**Solution:**
- Verify theme stylesheet path in `index.html`
- Clear browser cache and reload
- Check browser console for 404 errors on stylesheet loading

### Issue: Script Errors in Browser Console

**Solution:**
- Ensure script reference is added after other scripts
- Verify script path: `_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js`
- Check browser console for specific error messages

### Issue: Data Not Displaying in Cells

**Solution:**
- Verify DataSource binding: `DataSource="@HeatMapData"`
- Ensure data is initialized in `OnInitialized()` method
- Check that data format matches expected structure (2D array for array binding)
- Set `ShowLabel="true"` in `HeatMapCellSettings` to display cell values

### Performance Issues

**Solution:**
- For large datasets, consider pagination or limiting visible cells
- Disable animations if not needed
- Use production builds (`dotnet publish -c Release`) for better performance

## Next Steps

Now that you have a working HeatMap Chart, explore additional features:

- **Data Binding**: Learn different data source formats (array, JSON, cell-based)
- **Axis Configuration**: Customize x-axis and y-axis with labels and types
- **Appearance**: Customize colors, borders, titles, and labels
- **Palettes**: Apply gradient or fixed color schemes
- **Legends**: Add legends to explain color meanings
- **Tooltips**: Enable interactive tooltips for cell values
- **Events**: Handle user interactions like cell clicks
- **Bubble HeatMaps**: Create bubble visualizations with size and color
- **Accessibility**: Implement keyboard navigation and screen reader support
