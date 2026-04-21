# Getting Started with Blazor ProgressBar

## Table of Contents
- [Overview](#overview)
- [Prerequisites and System Requirements](#prerequisites-and-system-requirements)
- [Installation Methods](#installation-methods)
  - [Visual Studio Setup](#visual-studio-setup)
  - [Visual Studio Code Setup](#visual-studio-code-setup)
  - [.NET CLI Setup](#net-cli-setup)
- [Package Installation](#package-installation)
- [Configure the Application](#configure-the-application)
  - [Import Namespaces](#import-namespaces)
  - [Register Syncfusion Service](#register-syncfusion-service)
  - [Add Stylesheet and Script Resources](#add-stylesheet-and-script-resources)
- [Basic Implementation](#basic-implementation)
  - [Linear ProgressBar](#linear-progressbar)
  - [Circular ProgressBar](#circular-progressbar)
- [Verification](#verification)
- [Troubleshooting Installation](#troubleshooting-installation)

## Overview

This guide walks through setting up the Syncfusion Blazor ProgressBar component in a Blazor WebAssembly application. The ProgressBar component provides visual feedback for operations with determinate or indeterminate progress states.

The setup process involves:
1. Creating a Blazor WebAssembly application
2. Installing required NuGet packages
3. Configuring namespaces and services
4. Adding theme and script references
5. Implementing your first ProgressBar

## Prerequisites and System Requirements

Before starting, ensure you have:

- **Visual Studio 2022** (17.0 or later) OR **Visual Studio Code** (latest version)
- **.NET SDK 6.0 or later** - Verify installation:
  ```bash
  dotnet --version
  ```
- **Internet connection** for downloading NuGet packages
- **Basic knowledge** of Blazor and Razor syntax

**System Requirements:**
- Windows 10/11, macOS 10.15+, or Linux
- Minimum 4GB RAM (8GB recommended)
- 2GB free disk space

## Installation Methods

Choose one of three methods to create your Blazor WebAssembly application:

### Visual Studio Setup

**Step 1: Create New Project**
1. Open Visual Studio 2022
2. Click **Create a new project**
3. Search for "Blazor WebAssembly App"
4. Select **Blazor WebAssembly App** template
5. Click **Next**

**Step 2: Configure Project**
1. Enter **Project name** (e.g., `ProgressBarDemo`)
2. Choose **Location** for the project
3. Click **Next**

**Step 3: Additional Information**
1. Select **Framework**: .NET 6.0 or later
2. **Authentication type**: None (or as needed)
3. Click **Create**

Visual Studio will create the project structure and restore dependencies.

### Visual Studio Code Setup

**Step 1: Create Project via Terminal**
1. Open Visual Studio Code
2. Press <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) or <kbd>⌘</kbd>+<kbd>`</kbd> (macOS) to open integrated terminal
3. Navigate to your desired project location
4. Run:
   ```bash
   dotnet new blazorwasm -o ProgressBarDemo
   cd ProgressBarDemo
   ```

**Step 2: Open Project**
1. In VS Code, select **File → Open Folder**
2. Navigate to and select the `ProgressBarDemo` folder
3. Click **Select Folder**

VS Code will detect the project and may prompt to add required assets - click **Yes**.

### .NET CLI Setup

**Step 1: Verify .NET Installation**
```bash
dotnet --version
```
Ensure version is 6.0 or higher.

**Step 2: Create Blazor WebAssembly App**
```bash
dotnet new blazorwasm -o ProgressBarDemo
cd ProgressBarDemo
```

**Step 3: Verify Project Creation**
```bash
dotnet build
```
Should complete successfully with no errors.

## Package Installation

Install two required NuGet packages:
1. **Syncfusion.Blazor.ProgressBar** - ProgressBar component
2. **Syncfusion.Blazor.Themes** - Styling themes

### Using Visual Studio

1. Open **Tools → NuGet Package Manager → Manage NuGet Packages for Solution**
2. Click **Browse** tab
3. Search for `Syncfusion.Blazor.ProgressBar`
4. Select the package and click **Install**
5. Repeat for `Syncfusion.Blazor.Themes`
6. Accept license agreements if prompted

**Alternative: Package Manager Console**
```powershell
Install-Package Syncfusion.Blazor.ProgressBar -Version 25.1.35
Install-Package Syncfusion.Blazor.Themes -Version 25.1.35
```

### Using Visual Studio Code or .NET CLI

Open terminal in project directory and run:

```bash
dotnet add package Syncfusion.Blazor.ProgressBar
dotnet add package Syncfusion.Blazor.Themes
dotnet restore
```

**Verify Installation:**
Check the `.csproj` file - it should contain:
```xml
<ItemGroup>
  <PackageReference Include="Syncfusion.Blazor.ProgressBar" Version="25.1.35" />
  <PackageReference Include="Syncfusion.Blazor.Themes" Version="25.1.35" />
</ItemGroup>
```

## Configure the Application

### Import Namespaces

Open `_Imports.razor` file (located in project root) and add:

```razor
@using Syncfusion.Blazor
@using Syncfusion.Blazor.ProgressBar
```

This makes Syncfusion components available throughout your application without repeated using statements.

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
@using ProgressBarDemo
@using ProgressBarDemo.Shared
@using Syncfusion.Blazor
@using Syncfusion.Blazor.ProgressBar
```

### Register Syncfusion Service

Open `Program.cs` and register the Syncfusion Blazor service:

**For .NET 6.0+:**
```csharp
using Microsoft.AspNetCore.Components.Web;
using Microsoft.AspNetCore.Components.WebAssembly.Hosting;
using Syncfusion.Blazor;
using ProgressBarDemo;

var builder = WebAssemblyHostBuilder.CreateDefault(args);
builder.RootComponents.Add<App>("#app");
builder.RootComponents.Add<HeadOutlet>("head::after");

builder.Services.AddScoped(sp => new HttpClient 
{ 
    BaseAddress = new Uri(builder.HostEnvironment.BaseAddress) 
});

// Register Syncfusion Blazor service
builder.Services.AddSyncfusionBlazor();

await builder.Build().RunAsync();
```

The `AddSyncfusionBlazor()` method registers all required Syncfusion services.

### Add Stylesheet and Script Resources

Open `wwwroot/index.html` and add theme stylesheet and script references inside the `<head>` section:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no" />
    <title>ProgressBarDemo</title>
    <base href="/" />
    <link href="css/bootstrap/bootstrap.min.css" rel="stylesheet" />
    <link href="css/app.css" rel="stylesheet" />
    <link href="ProgressBarDemo.styles.css" rel="stylesheet" />
    
    <!-- Syncfusion Blazor Theme -->
    <link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
    
    <!-- Syncfusion Blazor Scripts -->
    <script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js" type="text/javascript"></script>
</head>
<body>
    <div id="app">Loading...</div>
    <script src="_framework/blazor.webassembly.js"></script>
</body>
</html>
```

**Available Themes:**
- `bootstrap5.css` - Bootstrap 5 theme (default)
- `material.css` - Material Design theme
- `fabric.css` - Fluent/Fabric theme
- `tailwind.css` - Tailwind CSS theme
- `fluent.css` - Fluent 2 theme
- `bootstrap4.css` - Bootstrap 4 theme
- `material3.css` - Material 3 theme

Change the theme by replacing `bootstrap5.css` with your preferred theme name.

**Using CDN (Alternative):**
```html
<!-- CDN References -->
<link href="https://cdn.syncfusion.com/blazor/25.1.35/styles/bootstrap5.css" rel="stylesheet" />
<script src="https://cdn.syncfusion.com/blazor/25.1.35/syncfusion-blazor.min.js" type="text/javascript"></script>
```

## Basic Implementation

### Linear ProgressBar

Create a simple linear progress bar in any Razor page (e.g., `Pages/Index.razor`):

```razor
@page "/"
@using Syncfusion.Blazor.ProgressBar

<h3>Linear ProgressBar Example</h3>

<SfProgressBar Type="ProgressType.Linear" 
               Value="50" 
               Minimum="0" 
               Maximum="100"
               Height="60"
               TrackThickness="12" 
               ProgressThickness="12">
</SfProgressBar>
```

**Explanation:**
- `Type="ProgressType.Linear"` - Creates horizontal progress bar
- `Value="50"` - Current progress (50%)
- `Minimum="0"` and `Maximum="100"` - Range from 0 to 100
- `Height="60"` - Bar height in pixels
- `TrackThickness="12"` - Background track thickness
- `ProgressThickness="12"` - Progress indicator thickness

### Circular ProgressBar

Add a circular progress indicator:

```razor
@page "/circular"
@using Syncfusion.Blazor.ProgressBar

<h3>Circular ProgressBar Example</h3>

<SfProgressBar Type="ProgressType.Circular" 
               Value="70" 
               Minimum="0" 
               Maximum="100"
               Height="160px"
               Width="160px"
               TrackThickness="8" 
               ProgressThickness="8">
</SfProgressBar>
```

**Explanation:**
- `Type="ProgressType.Circular"` - Creates circular/donut progress bar
- `Value="70"` - 70% completion
- `Height="160px"` and `Width="160px"` - Circle dimensions (include 'px' suffix)
- `TrackThickness="8"` - Circular track thickness
- `ProgressThickness="8"` - Progress arc thickness

### Complete Working Example

Here's a complete `Index.razor` with both types:

```razor
@page "/"
@using Syncfusion.Blazor.ProgressBar

<PageTitle>ProgressBar Demo</PageTitle>

<div style="padding: 20px;">
    <h3>Blazor ProgressBar Examples</h3>
    
    <div style="margin-bottom: 40px;">
        <h4>Linear ProgressBar</h4>
        <SfProgressBar Type="ProgressType.Linear" 
                       Value="50" 
                       Minimum="0" 
                       Maximum="100"
                       Height="60"
                       TrackThickness="12" 
                       ProgressThickness="12"
                       TrackColor="#e0e0e0"
                       ProgressColor="#0d6efd">
        </SfProgressBar>
    </div>
    
    <div style="margin-bottom: 40px;">
        <h4>Circular ProgressBar</h4>
        <SfProgressBar Type="ProgressType.Circular" 
                       Value="70" 
                       Minimum="0" 
                       Maximum="100"
                       Height="160px"
                       Width="160px"
                       TrackThickness="8" 
                       ProgressThickness="8"
                       TrackColor="#e0e0e0"
                       ProgressColor="#28a745">
        </SfProgressBar>
    </div>
</div>
```

## Verification

**Run the Application:**

**Visual Studio:**
- Press <kbd>F5</kbd> or click **Run** button
- Browser opens automatically with your app

**Visual Studio Code or CLI:**
```bash
dotnet run
```
Then open browser to `https://localhost:5001` (or port shown in terminal)

**Expected Result:**
- Linear progress bar showing 50% completion
- Circular progress bar showing 70% completion
- Both should render with smooth appearance and proper colors

## Troubleshooting Installation

### ProgressBar Not Visible

**Issue:** Component renders but nothing appears on screen

**Solutions:**
1. **Check theme reference** in `index.html`:
   ```html
   <link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
   ```

2. **Verify color contrast** - If background is white, try:
   ```razor
   <SfProgressBar ProgressColor="#0d6efd" TrackColor="#e0e0e0" ... />
   ```

3. **Check height/width** - Ensure adequate size:
   - Linear: `Height="60"` minimum
   - Circular: `Height="160px"` and `Width="160px"` minimum

### Build Errors

**Issue:** Compilation errors after adding package

**Solutions:**
1. **Clean and rebuild:**
   ```bash
   dotnet clean
   dotnet build
   ```

2. **Verify package installation** in `.csproj`:
   ```xml
   <PackageReference Include="Syncfusion.Blazor.ProgressBar" Version="25.1.35" />
   ```

3. **Check namespace imports** in `_Imports.razor`:
   ```razor
   @using Syncfusion.Blazor.ProgressBar
   ```

### Runtime Errors

**Issue:** "Service not registered" error

**Solution:** Ensure `Program.cs` includes:
```csharp
builder.Services.AddSyncfusionBlazor();
```

**Issue:** JavaScript errors in browser console

**Solution:** Verify script reference in `index.html`:
```html
<script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js"></script>
```

### Theme Not Applied

**Issue:** ProgressBar appears unstyled or has wrong colors

**Solutions:**
1. **Verify stylesheet path** matches your package version
2. **Clear browser cache** - Hard refresh (<kbd>Ctrl</kbd>+<kbd>F5</kbd>)
3. **Check CSS file exists** in `wwwroot/_content/Syncfusion.Blazor.Themes/`
4. **Try CDN reference** as alternative:
   ```html
   <link href="https://cdn.syncfusion.com/blazor/25.1.35/styles/bootstrap5.css" rel="stylesheet" />
   ```

### NuGet Package Errors

**Issue:** Cannot find or install Syncfusion packages

**Solutions:**
1. **Add Syncfusion NuGet source** (if not already added):
   ```bash
   dotnet nuget add source https://www.nuget.org/api/v2/ -n nuget.org
   ```

2. **Clear NuGet cache:**
   ```bash
   dotnet nuget locals all --clear
   ```

3. **Retry installation:**
   ```bash
   dotnet add package Syncfusion.Blazor.ProgressBar
   dotnet restore
   ```

## Next Steps

Now that you have a working ProgressBar:

1. **Explore Types** - Learn about Linear vs Circular types and when to use each
2. **Try States** - Implement indeterminate, buffer, active, and striped states
3. **Add Customization** - Apply colors, segments, thickness, and corner radius
4. **Implement Animations** - Configure smooth progress transitions
5. **Handle Events** - Respond to value changes and completion
6. **Add Annotations** - Display custom text and labels on the progress bar

Your ProgressBar component is ready to use. Start with the basic examples above and explore advanced features as needed for your application.
