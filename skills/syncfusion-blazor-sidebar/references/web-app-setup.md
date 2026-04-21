# Getting Started with Blazor Sidebar in Blazor Web App (.NET 8)

This guide covers installation and setup of the Syncfusion Blazor Sidebar component in a Blazor Web App with .NET 8.

## Table of Contents
- [Project Creation](#project-creation)
- [Install NuGet Packages](#install-nuget-packages)
- [Add Import Namespaces](#add-import-namespaces)
- [Register Syncfusion Service](#register-syncfusion-service)
- [Add Stylesheet and Script Resources](#add-stylesheet-and-script-resources)
- [Add Sidebar Component](#add-sidebar-component)
- [Render Mode Configuration](#render-mode-configuration)

## Project Creation

### Create in Visual Studio

Create a **Blazor Web App** using Visual Studio via Microsoft Templates or the Syncfusion® Blazor Extension. For detailed instructions, refer to Blazor Web App Getting Started documentation.

### Create in Visual Studio Code

Create a **Blazor Web App** using Visual Studio Code. For example, with the `Auto` interactive render mode:

```bash
dotnet new blazor -o BlazorWebApp -int Auto
cd BlazorWebApp
cd BlazorWebApp.Client
```

### Create via .NET CLI

```bash
dotnet new blazor -o BlazorWebApp -int Auto
cd BlazorWebApp
cd BlazorWebApp.Client
```

**Note:** Configure the appropriate Interactive render mode (`Auto`, `Server`, or `WebAssembly`) and Interactivity location when creating the Blazor Web App.

## Install NuGet Packages

Install `Syncfusion.Blazor.Navigations` and `Syncfusion.Blazor.Themes` NuGet packages in your project.

### Package Manager Console:

```bash
Install-Package Syncfusion.Blazor.Navigations -Version {{ site.releaseversion }}
Install-Package Syncfusion.Blazor.Themes -Version {{ site.releaseversion }}
```

### .NET CLI:

```bash
dotnet add package Syncfusion.Blazor.Navigations -Version {{ site.releaseversion }}
dotnet add package Syncfusion.Blazor.Themes -Version {{ site.releaseversion }}
```

**Important:** If using `WebAssembly` or `Auto` render modes in the Blazor Web App, install these packages in the **client project**.

All Syncfusion Blazor packages are available on nuget.org. See the NuGet packages documentation for details.

## Add Import Namespaces

After packages are installed, open `~/_Imports.razor` in the client project and add:

```csharp
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Navigations
```

## Register Syncfusion Service

Register the Syncfusion Blazor service in the **Program.cs** file:

```csharp
using Syncfusion.Blazor;

....
builder.Services.AddSyncfusionBlazor();
....
```

**Important:** If the **Interactive Render Mode** is set to `WebAssembly` or `Auto`, register the Syncfusion® Blazor service in **Program.cs** files of **both** the server and client projects in your Blazor Web App.

## Add Stylesheet and Script Resources

The theme stylesheet and script can be accessed from NuGet through Static Web Assets. Include stylesheet and script references in the `~/Components/App.razor` file:

```html
<link href="_content/Syncfusion.Blazor.Themes/fluent2.css" rel="stylesheet" />
....
<script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js" type="text/javascript"></script>
```

Check out the Blazor Themes documentation to discover various methods (Static Web Assets, CDN, and CRG) for referencing themes in your Blazor application. Also, check the Adding Script Reference documentation to learn different approaches for adding script references in your Blazor application.

## Add Sidebar Component

Add the Syncfusion Blazor Sidebar component in `~/Components/Pages/*.razor` file. If the interactivity location is set to `Per page/component` in the Web App, define a render mode at the top of the page (for example, `InteractiveServer`, `InteractiveWebAssembly` or `InteractiveAuto`).

**Note:** If the **Interactivity Location** is set to `Global` with `Auto` or `WebAssembly`, the render mode is automatically configured in the `App.razor` file by default.

Add render mode directive if needed:

```razor
@rendermode InteractiveAuto
```

Add the Sidebar component:

```razor
@using Syncfusion.Blazor.Navigations

<div id="header" style="height:45px;text-align: center;color:white;background-color:midnightblue;font-size:1.2rem;line-height:45px;">
    Header
</div>

<SfSidebar Width="250px">
    <ChildContent>
        <div style="text-align: center;" class="text-content">Sidebar</div>
    </ChildContent>
</SfSidebar>

<div class="text-content" style="text-align: center;">Main content</div>

<style>
    .e-sidebar {
        background-color: #f8f8f8;
        color: black;
    }

    .text-content {
        font-size: 1.5rem;
        padding: 3rem;
    }
</style>
```

## Render Mode Configuration

### Auto Render Mode

The `Auto` mode provides interactivity using the most appropriate technology based on browser capabilities:

```bash
dotnet new blazor -o BlazorWebApp -int Auto
```

### Server Render Mode

For server-side rendering with interactivity:

```bash
dotnet new blazor -o BlazorWebApp -int Server
```

### WebAssembly Render Mode

For client-side WebAssembly execution:

```bash
dotnet new blazor -o BlazorWebApp -int WebAssembly
```

Choose the render mode that best fits your application's requirements and deployment strategy.
