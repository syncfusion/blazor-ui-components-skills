# Getting Started with Blazor Timeline Component

## Table of Contents
- [Prerequisites](#prerequisites)
- [Create a Blazor WebAssembly App](#create-a-blazor-webassembly-app)
- [Install NuGet Packages](#install-nuget-packages)
- [Add Import Namespaces](#add-import-namespaces)
- [Register Syncfusion Service](#register-syncfusion-service)
- [Add Stylesheet and Script Resources](#add-stylesheet-and-script-resources)
- [Add Timeline Component](#add-timeline-component)
- [Run the Application](#run-the-application)

## Prerequisites

Before you start, ensure you have the following:
- [System requirements for Blazor components](url)
- Visual Studio, Visual Studio Code, or .NET CLI installed
- Basic knowledge of Blazor components

## Create a Blazor WebAssembly App

### Using Visual Studio

1. Create a **Blazor WebAssembly App** using Visual Studio
2. Use [Microsoft Templates](url) or the Syncfusion Blazor Extension
3. For detailed instructions, refer to the [Blazor WASM App Getting Started](url) documentation

### Using Visual Studio Code

1. Create a **Blazor WebAssembly App** using Visual Studio Code
2. Alternatively, use this command in the integrated terminal:

```bash
dotnet new blazorwasm -o BlazorApp
cd BlazorApp
```

### Using .NET CLI

1. Install the latest version of [.NET SDK](url)
2. Run the following command:

```bash
dotnet new blazorwasm -o BlazorApp
cd BlazorApp
```

## Install NuGet Packages

To add the **Blazor Timeline** component to your app, install the following NuGet packages:

- **Syncfusion.Blazor.Layouts** - Contains the Timeline component
- **Syncfusion.Blazor.Themes** - Provides theme stylesheets

### Using NuGet Package Manager (Visual Studio)

1. Open NuGet Package Manager: *Tools → NuGet Package Manager → Manage NuGet Packages for Solution*
2. Search for `Syncfusion.Blazor.Layouts`
3. Search for `Syncfusion.Blazor.Themes`
4. Click Install for both packages

### Using Package Manager Console

```csharp
Install-Package Syncfusion.Blazor.Layouts -Version {{ site.releaseversion }}
Install-Package Syncfusion.Blazor.Themes -Version {{ site.releaseversion }}
```

### Using .NET CLI

```bash
dotnet add package Syncfusion.Blazor.Layouts -v {{ site.releaseversion }}
dotnet add package Syncfusion.Blazor.Themes -v {{ site.releaseversion }}
dotnet restore
```

> **Note**: Syncfusion Blazor components are available on [nuget.org](url). Refer to the [NuGet packages](url) topic for the available NuGet packages list.

## Add Import Namespaces

Open the **~/_Imports.razor** file in your Blazor application and add the following using statements:

```csharp
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Layouts
```

This makes the Timeline component and related classes available throughout your application.

## Register Syncfusion Service

Register the Syncfusion Blazor Service in the **~/Program.cs** file:

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

## Add Stylesheet and Script Resources

The theme stylesheet and script can be accessed from NuGet through Static Web Assets. Include the stylesheet and script references in the `<head>` section of the **~/index.html** file:

```html
<head>
    ....
    <link href="_content/Syncfusion.Blazor.Themes/material3.css" rel="stylesheet" />
    <script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js" type="text/javascript"></script>
</head>
```

> **Alternative Theme Options**: You can use different themes by replacing `material3.css` with other available themes like `bootstrap5.css`, `tailwind.css`, or `fluent.css`.

> **Note**: For more details on themes and other resource inclusion methods, check out the [Blazor Themes](url) topic and [Adding Script Reference](url) documentation.

## Add Timeline Component

Add the Syncfusion Blazor Timeline component in your component file (e.g., **~/Pages/Index.razor**):

```razor
@using Syncfusion.Blazor.Layouts

<div style="height: 250px;">
    <SfTimeline>
        <TimelineItems>
            <TimelineItem>
                <Content>Shipped</Content>
            </TimelineItem>
            <TimelineItem>
                <Content>Departed</Content>
            </TimelineItem>
            <TimelineItem>
                <Content>Arrived</Content>
            </TimelineItem>
            <TimelineItem>
                <Content>Out for Delivery</Content>
            </TimelineItem>
        </TimelineItems>
    </SfTimeline>
</div>
```

## Run the Application

Press **Ctrl+F5** (Windows) or **⌘+F5** (macOS) to launch the application. The Syncfusion Blazor Timeline component will render in your default web browser.

### What You Should See

- A vertical timeline with four items
- Each item displays the content ("Shipped", "Departed", etc.)
- A visual connector line between items
- A dot indicator for each item

## Next Steps

Once your basic Timeline is working:

1. **Explore Alignment**: Use different alignment modes (Before, After, Alternate, AlternateReverse)
2. **Add Opposite Content**: Display additional information opposite to the main content
3. **Change Orientation**: Switch between vertical and horizontal layouts
4. **Customize Appearance**: Apply custom CSS to dots, connectors, and items
5. **Handle Events**: Use Created and ItemRendered events for component lifecycle management

## Troubleshooting

- **Theme not loading**: Verify the stylesheet path in index.html matches your installed package version
- **Component not rendering**: Ensure `@using Syncfusion.Blazor.Layouts` is in _Imports.razor
- **Service errors**: Confirm `AddSyncfusionBlazor()` is called in Program.cs
- **Build errors**: Reinstall NuGet packages with correct versions
