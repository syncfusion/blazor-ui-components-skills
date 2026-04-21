# Getting Started with Blazor Carousel

## Table of Contents
- [Install NuGet Packages](#install-nuget-packages)
- [Add Namespaces](#add-namespaces)
- [Register Syncfusion Service](#register-syncfusion-service)
- [Add Stylesheet and Script References](#add-stylesheet-and-script-references)
- [Add Carousel Component](#add-carousel-component)

## Install NuGet Packages

Install the required Syncfusion Blazor packages in your project:

**Using Package Manager Console:**
```bash
Install-Package Syncfusion.Blazor.Navigations -Version {{ site.releaseversion }}
Install-Package Syncfusion.Blazor.Themes -Version {{ site.releaseversion }}
```

**Using .NET CLI:**
```bash
dotnet add package Syncfusion.Blazor.Navigations
dotnet add package Syncfusion.Blazor.Themes
```

**Using NuGet Package Manager:**
Navigate to *Tools → NuGet Package Manager → Manage NuGet Packages for Solution* and search for `Syncfusion.Blazor.Navigations` and `Syncfusion.Blazor.Themes`.

All Syncfusion Blazor packages are available on [nuget.org](https://www.nuget.org/packages?q=syncfusion.blazor).

## Add Namespaces

Open the `~/_Imports.razor` file and add the following namespaces:

```cshtml
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Navigations
```

This makes the Syncfusion Blazor components available throughout your application without needing to add the namespace to each component file.

## Register Syncfusion Service

Register the Syncfusion Blazor Service in the `Program.cs` file:

```csharp
using Syncfusion.Blazor;

var builder = WebAssemblyHostBuilder.CreateDefault(args);
// ... other configurations

builder.Services.AddSyncfusionBlazor();

await builder.Build().RunAsync();
```

This service registration is required for all Syncfusion Blazor components to function properly.

## Add Stylesheet and Script References

Add the theme stylesheet and script references in the `~/index.html` file (for Blazor WebAssembly) or `~/Pages/_Host.cshtml` file (for Blazor Server):

```html
<head>
    <!-- Other head content -->
    <link href="_content/Syncfusion.Blazor.Themes/fluent2.css" rel="stylesheet" />
</head>

<body>
    <!-- Other body content -->
    <script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js" type="text/javascript"></script>
</body>
```

**Available Themes:**
- `bootstrap5.css`
- `material3.css`
- `fluent2.css`
- `tailwind.css`
- `fluent.css`
- `material.css`
- `bootstrap4.css`
- `bootstrap.css`
- `fabric.css`
- `highcontrast.css`

You can also reference themes via CDN or use the Custom Resource Generator (CRG) for optimized theme bundles.

## Add Carousel Component

Add the Syncfusion Blazor Carousel component to a Razor page (e.g., `~/Pages/Index.razor`):

```cshtml
@using Syncfusion.Blazor.Navigations

<SfCarousel>
    <CarouselItem>
        <figure class="img-container">
            <img src="https://cdn.syncfusion.com/blazor/images/carousel/bridge.png" 
                 alt="Golden Gate Bridge, San Francisco" 
                 style="height:100%;width:100%;" />
            <figcaption class="img-caption">Golden Gate Bridge, San Francisco</figcaption>
        </figure>
    </CarouselItem>
    <CarouselItem>
        <figure class="img-container">
            <img src="https://cdn.syncfusion.com/blazor/images/carousel/trees.png" 
                 alt="Spring Flower Trees" 
                 style="height:100%;width:100%;" />
            <figcaption class="img-caption">Spring Flower Trees</figcaption>
        </figure>
    </CarouselItem>
    <CarouselItem>
        <figure class="img-container">
            <img src="https://cdn.syncfusion.com/blazor/images/carousel/waterfall.png" 
                 alt="Oddadalen Waterfalls, Norway" 
                 style="height:100%;width:100%;" />
            <figcaption class="img-caption">Oddadalen Waterfalls, Norway</figcaption>
        </figure>
    </CarouselItem>
    <CarouselItem>
        <figure class="img-container">
            <img src="https://cdn.syncfusion.com/blazor/images/carousel/sea.png" 
                 alt="Anse Source d'Argent, Seychelles" 
                 style="height:100%;width:100%;" />
            <figcaption class="img-caption">Anse Source d'Argent, Seychelles</figcaption>
        </figure>
    </CarouselItem>
    <CarouselItem>
        <figure class="img-container">
            <img src="https://cdn.syncfusion.com/blazor/images/carousel/rocks.png" 
                 alt="Stonehenge, England" 
                 style="height:100%;width:100%;" />
            <figcaption class="img-caption">Stonehenge, England</figcaption>
        </figure>
    </CarouselItem>
</SfCarousel>

<style>
    .e-carousel .e-carousel-items .e-carousel-item .img-container {
        height: 100%;
    }

    .e-carousel .e-carousel-items .e-carousel-item .img-caption {
        bottom: 4em;
        color: #fff;
        font-size: 12pt;
        height: 2em;
        position: relative;
        padding: 0.3em 1em;
        text-align: center;
        width: 100%;
    }
</style>
```

Run the application using <kbd>Ctrl</kbd>+<kbd>F5</kbd> (Windows) or <kbd>⌘</kbd>+<kbd>F5</kbd> (macOS) to see the Carousel component in action.

## Basic Features

The default carousel includes:
- **Automatic slide transitions** every 5 seconds
- **Previous/Next navigation buttons**
- **Dot indicators** showing total slides and current position
- **Touch swipe support** for mobile devices
- **Keyboard navigation** using arrow keys
- **Infinite looping** when reaching the last slide

## Next Steps

- Configure item selection → See [Populating Items](populating-items.md)
- Customize navigators and indicators → See [Navigators and Indicators](navigators-and-indicators.md)
- Add animations → See [Animations and Transitions](animations-and-transitions.md)
- Ensure accessibility → See [Accessibility](accessibility.md)
- Apply custom styling → See [Styles and Appearance](styles-and-appearance.md)
