# Getting Started with Blazor Sidebar

This guide covers installation and setup of the Syncfusion Blazor Sidebar component across Visual Studio, Visual Studio Code, and .NET CLI.

## Table of Contents
- [Visual Studio Setup](#visual-studio-setup)
- [Visual Studio Code Setup](#visual-studio-code-setup)
- [NET CLI Setup](#net-cli-setup)
- [Namespaces and Service Registration](#namespaces-and-service-registration)
- [Theme and Script Resources](#theme-and-script-resources)
- [Basic Component Rendering](#basic-component-rendering)
- [Enable Backdrop](#enable-backdrop)

## Visual Studio Setup

### Prerequisites

* System requirements for Blazor components

### Create a new Blazor App in Visual Studio

Create a **Blazor WebAssembly App** using Visual Studio via Microsoft Templates or the Syncfusion® Blazor Extension.

### Install NuGet Packages

Open the NuGet package manager in Visual Studio (*Tools → NuGet Package Manager → Manage NuGet Packages for Solution*), then search and install:
- `Syncfusion.Blazor.Navigations`
- `Syncfusion.Blazor.Themes`

Alternatively, run in Package Manager Console:

```csharp
Install-Package Syncfusion.Blazor.Navigations -Version {{ site.releaseversion }}
Install-Package Syncfusion.Blazor.Themes -Version {{ site.releaseversion }}
```

## Visual Studio Code Setup

### Prerequisites

* System requirements for Blazor components

### Create a new Blazor App in VS Code

Create a **Blazor WebAssembly App** using Visual Studio Code or run in integrated terminal:

```bash
dotnet new blazorwasm -o BlazorApp
cd BlazorApp
```

### Install NuGet Packages

Press `Ctrl+`` to open integrated terminal. Ensure you're in project root directory where `.csproj` is located:

```bash
dotnet add package Syncfusion.Blazor.Navigations -v {{ site.releaseversion }}
dotnet add package Syncfusion.Blazor.Themes -v {{ site.releaseversion }}
dotnet restore
```

## .NET CLI Setup

### Prerequisites

Install latest .NET SDK. Check your version:

```bash
dotnet --version
```

### Create a Blazor WebAssembly App

```bash
dotnet new blazorwasm -o BlazorApp
cd BlazorApp
```

### Install NuGet Packages

```bash
dotnet add package Syncfusion.Blazor.Navigations -Version {{ site.releaseversion }}
dotnet add package Syncfusion.Blazor.Themes -Version {{ site.releaseversion }}
dotnet restore
```

## Namespaces and Service Registration

### Add Import Namespaces

Open the `~/_Imports.razor` file and add:

```csharp
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Navigations
```

### Register Syncfusion Blazor Service

In `~/Program.cs`:

```csharp
using Microsoft.AspNetCore.Components.Web;
using Microsoft.AspNetCore.Components.WebAssembly.Hosting;
using Syncfusion.Blazor;

var builder = WebAssemblyHostBuilder.CreateDefault(args);
builder.RootComponents.Add<App>("#app");
builder.RootComponents.Add<HeadOutlet>("head::after");

builder.Services.AddScoped(sp => new HttpClient { BaseAddress = new Uri(builder.HostEnvironment.BaseAddress) });

builder.Services.AddSyncfusionBlazor();
await builder.Build().RunAsync();
```

## Theme and Script Resources

The theme stylesheet and script can be accessed from NuGet through Static Web Assets. Include stylesheet and script references within the `<head>` section of `~/index.html`:

```html
<head>
    ....
    <link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
    <script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js" type="text/javascript"></script>
</head>
```

## Basic Component Rendering

Add the Syncfusion Blazor Sidebar component in `~/Pages/Index.razor`:

```razor
<div id="header" style="height:45px;text-align: center;color:white;background-color:midnightblue;font-size:1.2rem;line-height:45px;">
    Header
</div>

<SfSidebar Width="250px">
    <ChildContent>
        <div style="text-align: center;" class="text-content"> Sidebar </div>
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

Press `Ctrl+F5` (Windows) or `⌘+F5` (macOS) to launch the application.

## Enable Backdrop

Enabling the `ShowBackdrop` property in the Sidebar component prevents the main content from user interactions:

```razor
<SfSidebar Width="250px" ShowBackdrop="true">
    <ChildContent>
        <div style="text-align: center;" class="text-content"> Sidebar </div>
    </ChildContent>
</SfSidebar>
```

The backdrop creates an overlay that blocks clicks and interactions on the main content while the sidebar is open, improving focus on the sidebar navigation.
