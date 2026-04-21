# Getting Started with Blazor Sankey Diagram

This guide provides complete installation and setup instructions for implementing the Syncfusion Blazor Sankey Diagram component in Blazor Server, Blazor WebAssembly, and Blazor Web App (.NET 8+) projects.

## Overview

The Blazor Sankey Diagram is a flow visualization component that displays relationships between categories using proportionally-sized links. The component requires:
- **NuGet Package**: Syncfusion.Blazor.Sankey
- **Namespace**: Syncfusion.Blazor.Sankey
- **Service Registration**: AddSyncfusionBlazor()
- **Theme CSS**: Syncfusion theme stylesheet
- **Scripts**: syncfusion-blazor.min.js

## Prerequisites

- Visual Studio 2022 or Visual Studio Code
- .NET 6.0 SDK or later (.NET 8.0 recommended for Web App)
- Basic knowledge of Blazor and C#
- [System requirements for Blazor components](https://blazor.syncfusion.com/documentation/system-requirements)

---

## Installation for Blazor Server App

### Step 1: Create Blazor Server Project

**Using Visual Studio:**
1. Open Visual Studio 2022
2. Create new project → **Blazor Server App**
3. Configure project name and location
4. Select .NET version (6.0, 7.0, or 8.0)
5. Click **Create**

**Using .NET CLI:**
```bash
dotnet new blazorserver -o MyBlazorApp
cd MyBlazorApp
```

### Step 2: Install NuGet Package

**Option 1 - Visual Studio Package Manager:**
1. Tools → NuGet Package Manager → Manage NuGet Packages for Solution
2. Browse tab → Search for "Syncfusion.Blazor.Sankey"
3. Select latest version and click **Install**

**Option 2 - Package Manager Console:**
```powershell
Install-Package Syncfusion.Blazor.Sankey
```

**Option 3 - .NET CLI:**
```bash
dotnet add package Syncfusion.Blazor.Sankey
dotnet restore
```

### Step 3: Import Namespaces

Open `_Imports.razor` and add:

```razor
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Sankey
```

### Step 4: Register Syncfusion Service

Open `Program.cs` and register the service:

```csharp
using Syncfusion.Blazor;

var builder = WebApplication.CreateBuilder(args);

// Add services
builder.Services.AddRazorPages();
builder.Services.AddServerSideBlazor();
builder.Services.AddSyncfusionBlazor(); // Add this line

var app = builder.Build();
// ... rest of configuration
```

### Step 5: Add Theme and Scripts

**For .NET 6:**  
Open `Pages/_Layout.cshtml` and add in the `<head>`:

```html
<head>
    <link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
    <script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js" type="text/javascript"></script>
</head>
```

**For .NET 7:**  
Open `Pages/_Host.cshtml` and add the same references.

**For .NET 8:**  
Open `Components/App.razor` and add in the `<head>`:

```html
<head>
    <link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
    <script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js" type="text/javascript"></script>
</head>
```

### Step 6: Add Sankey Component

Create or open a `.razor` file (e.g., `Pages/Index.razor`) and add:

```razor
@page "/"
@using Syncfusion.Blazor.Sankey

<h3>Coffee Production Flow</h3>

<SfSankey Nodes="@Nodes" Links="@Links" Width="100%" Height="600px">
</SfSankey>

@code {
    public List<SankeyDataNode> Nodes = new List<SankeyDataNode>();
    public List<SankeyDataLink> Links = new List<SankeyDataLink>();

    protected override void OnInitialized()
    {
        Nodes = new List<SankeyDataNode>()
        {
            new SankeyDataNode() { Id = "Coffee Production" },
            new SankeyDataNode() { Id = "Arabica" },
            new SankeyDataNode() { Id = "Robusta" },
            new SankeyDataNode() { Id = "Roasted Coffee" },
            new SankeyDataNode() { Id = "North America" },
            new SankeyDataNode() { Id = "Europe" },
            new SankeyDataNode() { Id = "Asia Pacific" }
        };

        Links = new List<SankeyDataLink>()
        {
            new SankeyDataLink() { SourceId = "Coffee Production", TargetId = "Arabica", Value = 95 },
            new SankeyDataLink() { SourceId = "Coffee Production", TargetId = "Robusta", Value = 65 },
            new SankeyDataLink() { SourceId = "Arabica", TargetId = "Roasted Coffee", Value = 60 },
            new SankeyDataLink() { SourceId = "Robusta", TargetId = "Roasted Coffee", Value = 30 },
            new SankeyDataLink() { SourceId = "Roasted Coffee", TargetId = "North America", Value = 35 },
            new SankeyDataLink() { SourceId = "Roasted Coffee", TargetId = "Europe", Value = 30 },
            new SankeyDataLink() { SourceId = "Roasted Coffee", TargetId = "Asia Pacific", Value = 25 }
        };
    }
}
```

### Step 7: Run the Application

Press **Ctrl+F5** (Windows) or **⌘+F5** (macOS) to run without debugging.

---

## Installation for Blazor WebAssembly App

### Step 1: Create Blazor WebAssembly Project

**Using Visual Studio:**
1. Open Visual Studio 2022
2. Create new project → **Blazor WebAssembly App**
3. Configure project name and location
4. Select .NET version
5. Click **Create**

**Using .NET CLI:**
```bash
dotnet new blazorwasm -o MyBlazorWasmApp
cd MyBlazorWasmApp
```

### Step 2: Install NuGet Package

Same as Blazor Server - use any of these methods:

```bash
# .NET CLI
dotnet add package Syncfusion.Blazor.Sankey
dotnet restore
```

```powershell
# Package Manager Console
Install-Package Syncfusion.Blazor.Sankey
```

### Step 3: Import Namespaces

Open `_Imports.razor` and add:

```razor
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Sankey
```

### Step 4: Register Syncfusion Service

Open `Program.cs` and register:

```csharp
using Microsoft.AspNetCore.Components.Web;
using Microsoft.AspNetCore.Components.WebAssembly.Hosting;
using Syncfusion.Blazor;

var builder = WebAssemblyHostBuilder.CreateDefault(args);
builder.RootComponents.Add<App>("#app");
builder.RootComponents.Add<HeadOutlet>("head::after");

builder.Services.AddScoped(sp => new HttpClient { BaseAddress = new Uri(builder.HostEnvironment.BaseAddress) });
builder.Services.AddSyncfusionBlazor(); // Add this line

await builder.Build().RunAsync();
```

### Step 5: Add Theme and Scripts

Open `wwwroot/index.html` and add in the `<head>`:

```html
<head>
    <link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
    <script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js" type="text/javascript"></script>
</head>
```

### Step 6: Add Sankey Component

Use the same component code as Blazor Server (Step 6 above).

---

## Installation for Blazor Web App (.NET 8+)

Blazor Web App (.NET 8) supports multiple render modes and requires careful configuration.

### Step 1: Create Blazor Web App

**Using Visual Studio 2022:**
1. Create new project → **Blazor Web App**
2. Configure project name
3. **Important**: Select Interactive Render Mode:
   - **Server**: Server-side only
   - **WebAssembly**: Client-side only
   - **Auto**: Server initially, then WebAssembly
4. Select Interactivity Location:
   - **Global**: All components interactive
   - **Per page/component**: Add @rendermode per component
5. Click **Create**

**Using .NET CLI:**
```bash
# Auto render mode (recommended)
dotnet new blazor -o MyBlazorWebApp -int Auto
cd MyBlazorWebApp
```

```bash
# Server render mode
dotnet new blazor -o MyBlazorWebApp -int Server

# WebAssembly render mode
dotnet new blazor -o MyBlazorWebApp -int WebAssembly
```

### Step 2: Install NuGet Package

**For WebAssembly or Auto render mode:**  
Install in the **Client project** (MyBlazorWebApp.Client):

```bash
cd MyBlazorWebApp.Client
dotnet add package Syncfusion.Blazor.Sankey
dotnet restore
cd ..
```

**For Server render mode only:**  
Install in the main project:

```bash
dotnet add package Syncfusion.Blazor.Sankey
dotnet restore
```

### Step 3: Import Namespaces

**For WebAssembly or Auto:**  
Open `MyBlazorWebApp.Client/_Imports.razor`:

```razor
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Sankey
```

**For Server only:**  
Open `_Imports.razor` in the main project.

### Step 4: Register Syncfusion Service

**For WebAssembly or Auto** - Register in BOTH projects:

**Server Project (`Program.cs`):**
```csharp
using Syncfusion.Blazor;

var builder = WebApplication.CreateBuilder(args);

builder.Services.AddRazorComponents()
    .AddInteractiveServerComponents()
    .AddInteractiveWebAssemblyComponents();

builder.Services.AddSyncfusionBlazor(); // Add this

var app = builder.Build();
// ... rest of configuration
```

**Client Project (`MyBlazorWebApp.Client/Program.cs`):**
```csharp
using Microsoft.AspNetCore.Components.WebAssembly.Hosting;
using Syncfusion.Blazor;

var builder = WebAssemblyHostBuilder.CreateDefault(args);

builder.Services.AddSyncfusionBlazor(); // Add this

await builder.Build().RunAsync();
```

**For Server only:**  
Register only in the main `Program.cs` as shown above.

### Step 5: Add Theme and Scripts

Open `Components/App.razor` and add in the `<head>`:

```html
<head>
    <link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
    <script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js" type="text/javascript"></script>
</head>
```

### Step 6: Add Sankey Component with Render Mode

**If Interactivity Location = "Per page/component":**

Add render mode directive at the top of your component:

```razor
@page "/sankey"
@rendermode InteractiveAuto
@using Syncfusion.Blazor.Sankey

<h3>Energy Flow Diagram</h3>

<SfSankey Nodes="@Nodes" Links="@Links" Width="100%" Height="600px">
</SfSankey>

@code {
    public List<SankeyDataNode> Nodes = new List<SankeyDataNode>();
    public List<SankeyDataLink> Links = new List<SankeyDataLink>();

    protected override void OnInitialized()
    {
        Nodes = new List<SankeyDataNode>()
        {
            new SankeyDataNode() { Id = "Solar" },
            new SankeyDataNode() { Id = "Wind" },
            new SankeyDataNode() { Id = "Electricity" },
            new SankeyDataNode() { Id = "Residential" },
            new SankeyDataNode() { Id = "Commercial" }
        };

        Links = new List<SankeyDataLink>()
        {
            new SankeyDataLink() { SourceId = "Solar", TargetId = "Electricity", Value = 30 },
            new SankeyDataLink() { SourceId = "Wind", TargetId = "Electricity", Value = 45 },
            new SankeyDataLink() { SourceId = "Electricity", TargetId = "Residential", Value = 35 },
            new SankeyDataLink() { SourceId = "Electricity", TargetId = "Commercial", Value = 40 }
        };
    }
}
```

**Render Mode Options:**
- `@rendermode InteractiveServer` - Server-side only
- `@rendermode InteractiveWebAssembly` - Client-side only
- `@rendermode InteractiveAuto` - Server first, then WebAssembly (recommended)

**If Interactivity Location = "Global":**  
No need to add `@rendermode` directive.

---

## Available Themes

Replace the theme CSS in your project with one of these:

- `bootstrap5.css` - Bootstrap 5 (default)
- `material.css` - Material Design
- `material-dark.css` - Material Dark
- `fluent.css` - Microsoft Fluent
- `fluent-dark.css` - Fluent Dark
- `tailwind.css` - Tailwind CSS
- `tailwind-dark.css` - Tailwind Dark
- `fabric.css` - Office Fabric
- `fabric-dark.css` - Fabric Dark

**Example:**
```html
<link href="_content/Syncfusion.Blazor.Themes/fluent.css" rel="stylesheet" />
```

---

## Complete Minimal Example

Here's a complete, copy-paste ready example:

```razor
@page "/sankey-demo"
@using Syncfusion.Blazor.Sankey

<h3>Global Coffee Production Flow</h3>

<SfSankey Nodes="@Nodes" Links="@Links" Width="100%" Height="600px">
    <SankeyNodeSettings Width="20" Padding="20"></SankeyNodeSettings>
    <SankeyLinkSettings Opacity="0.4"></SankeyLinkSettings>
    <SankeyLabelSettings Visible="true" FontSize="14px"></SankeyLabelSettings>
    <SankeyTooltipSettings Enable="true"></SankeyTooltipSettings>
    <SankeyLegendSettings Visible="true" Position="LegendPosition.Bottom"></SankeyLegendSettings>
</SfSankey>

@code {
    public List<SankeyDataNode> Nodes = new List<SankeyDataNode>();
    public List<SankeyDataLink> Links = new List<SankeyDataLink>();

    protected override void OnInitialized()
    {
        // Define nodes (categories/stages)
        Nodes = new List<SankeyDataNode>()
        {
            new SankeyDataNode() { Id = "Coffee Production", Label = new SankeyDataLabel() { Text = "Coffee Production" } },
            new SankeyDataNode() { Id = "Arabica", Label = new SankeyDataLabel() { Text = "Arabica" } },
            new SankeyDataNode() { Id = "Robusta", Label = new SankeyDataLabel() { Text = "Robusta" } },
            new SankeyDataNode() { Id = "Roasted Coffee", Label = new SankeyDataLabel() { Text = "Roasted" } },
            new SankeyDataNode() { Id = "Instant Coffee", Label = new SankeyDataLabel() { Text = "Instant" } },
            new SankeyDataNode() { Id = "Green Coffee", Label = new SankeyDataLabel() { Text = "Green" } },
            new SankeyDataNode() { Id = "North America", Label = new SankeyDataLabel() { Text = "North America" } },
            new SankeyDataNode() { Id = "Europe", Label = new SankeyDataLabel() { Text = "Europe" } },
            new SankeyDataNode() { Id = "Asia Pacific", Label = new SankeyDataLabel() { Text = "Asia Pacific" } }
        };

        // Define links (flows with magnitude)
        Links = new List<SankeyDataLink>()
        {
            // Production to types
            new SankeyDataLink() { SourceId = "Coffee Production", TargetId = "Arabica", Value = 95 },
            new SankeyDataLink() { SourceId = "Coffee Production", TargetId = "Robusta", Value = 65 },
            
            // Types to processing
            new SankeyDataLink() { SourceId = "Arabica", TargetId = "Roasted Coffee", Value = 60 },
            new SankeyDataLink() { SourceId = "Arabica", TargetId = "Instant Coffee", Value = 20 },
            new SankeyDataLink() { SourceId = "Arabica", TargetId = "Green Coffee", Value = 15 },
            new SankeyDataLink() { SourceId = "Robusta", TargetId = "Roasted Coffee", Value = 30 },
            new SankeyDataLink() { SourceId = "Robusta", TargetId = "Instant Coffee", Value = 25 },
            new SankeyDataLink() { SourceId = "Robusta", TargetId = "Green Coffee", Value = 10 },
            
            // Processing to regions
            new SankeyDataLink() { SourceId = "Roasted Coffee", TargetId = "North America", Value = 35 },
            new SankeyDataLink() { SourceId = "Roasted Coffee", TargetId = "Europe", Value = 30 },
            new SankeyDataLink() { SourceId = "Roasted Coffee", TargetId = "Asia Pacific", Value = 25 },
            new SankeyDataLink() { SourceId = "Instant Coffee", TargetId = "North America", Value = 15 },
            new SankeyDataLink() { SourceId = "Instant Coffee", TargetId = "Europe", Value = 15 },
            new SankeyDataLink() { SourceId = "Instant Coffee", TargetId = "Asia Pacific", Value = 15 },
            new SankeyDataLink() { SourceId = "Green Coffee", TargetId = "North America", Value = 10 },
            new SankeyDataLink() { SourceId = "Green Coffee", TargetId = "Europe", Value = 8 },
            new SankeyDataLink() { SourceId = "Green Coffee", TargetId = "Asia Pacific", Value = 7 }
        };
    }
}
```

---

## Troubleshooting

### Component Not Rendering

**Symptom**: Sankey diagram doesn't appear on the page.

**Solutions**:
1. **Check service registration**: Ensure `AddSyncfusionBlazor()` is called in `Program.cs`
2. **Verify namespaces**: Confirm `@using Syncfusion.Blazor.Sankey` in `_Imports.razor`
3. **Set dimensions**: Add `Width` and `Height` to `<SfSankey>`:
   ```razor
   <SfSankey Width="100%" Height="600px" Nodes="@Nodes" Links="@Links">
   ```
4. **Check data**: Ensure `Nodes` and `Links` collections are initialized
5. **For .NET 8 Web App**: Add `@rendermode` directive if interactivity is per-page

### Script or Style Not Loading

**Symptom**: Console errors about missing scripts or styles.

**Solutions**:
1. **Verify script reference**: Check `syncfusion-blazor.min.js` is in the correct file:
   - Blazor Server (.NET 6): `_Layout.cshtml`
   - Blazor Server (.NET 7): `_Host.cshtml`
   - Blazor Server (.NET 8): `App.razor`
   - Blazor WASM: `index.html`
   - Blazor Web App: `App.razor`

2. **Verify theme CSS**: Check theme CSS is referenced before the script
3. **Build project**: Clean and rebuild the solution
4. **Check static web assets**: Ensure packages were restored correctly

### NuGet Package Not Found

**Symptom**: Package manager can't find Syncfusion.Blazor.Sankey.

**Solutions**:
1. **Check package source**: Ensure nuget.org is in package sources
2. **Update package manager**: Tools → Options → NuGet Package Manager → Clear All NuGet Cache(s)
3. **Use specific version**: Try installing with explicit version number
4. **Check internet connection**: Verify access to nuget.org

### Render Mode Issues (.NET 8 Web App)

**Symptom**: Component interactive features not working in .NET 8 Web App.

**Solutions**:
1. **Add render mode**: If interactivity location is "Per page/component", add:
   ```razor
   @rendermode InteractiveAuto
   ```
2. **Check both projects**: For WebAssembly/Auto, ensure service registered in both server and client projects
3. **Verify package location**: For WebAssembly/Auto, install package in the `.Client` project

### License Warning

**Symptom**: License warning message displayed.

**Solution**: Register your license key in `Program.cs` before building:

```csharp
// Add this before builder.Build()
Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense("YOUR-LICENSE-KEY");
```

Get a free community license or trial at: https://www.syncfusion.com/sales/products

---

## Next Steps

After successful installation:
1. Explore data binding patterns for dynamic data
2. Customize node and link appearance
3. Add interactive events for user interaction
4. Configure tooltips and legends
5. Implement print and export functionality

## Resources

- **Official Documentation**: https://blazor.syncfusion.com/documentation/sankey-chart/getting-started
- **Live Demos**: https://blazor.syncfusion.com/demos/sankey-chart/default-functionalities
- **API Reference**: https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.Sankey.html
- **Support**: https://www.syncfusion.com/support/
