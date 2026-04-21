# Getting Started with Blazor Media Query

## Table of Contents
- [NuGet Installation](#nuget-installation)
- [Namespace Imports](#namespace-imports)
- [Service Registration](#service-registration)
- [Theme Stylesheet](#theme-stylesheet)
- [Basic Component Usage](#basic-component-usage)

## NuGet Installation

Install the Syncfusion.Blazor.Core and Syncfusion.Blazor.Themes packages using your preferred method.

### Visual Studio Package Manager
```
Install-Package Syncfusion.Blazor.Core
Install-Package Syncfusion.Blazor.Themes
```

### .NET CLI
```
dotnet add package Syncfusion.Blazor.Core
dotnet add package Syncfusion.Blazor.Themes
dotnet restore
```

### Visual Studio Code Terminal
```
dotnet add package Syncfusion.Blazor.Core
dotnet add package Syncfusion.Blazor.Themes
dotnet restore
```

## Namespace Imports

Open the `~/_Imports.razor` file and add the Syncfusion.Blazor namespace:

```razor
@using Syncfusion.Blazor
```

This imports all Syncfusion Blazor components including SfMediaQuery.

## Service Registration

Register the Syncfusion Blazor Service in the `~/Program.cs` file:

```csharp
using Syncfusion.Blazor;

var builder = WebAssemblyHostBuilder.CreateDefault(args);
builder.RootComponents.Add<App>("#app");
builder.RootComponents.Add<HeadOutlet>("head::after");

builder.Services.AddScoped(sp => new HttpClient 
{ 
    BaseAddress = new Uri(builder.HostEnvironment.BaseAddress) 
});

// Register Syncfusion Blazor Service
builder.Services.AddSyncfusionBlazor();

await builder.Build().RunAsync();
```

## Theme Stylesheet

Include the Syncfusion theme stylesheet in the `<head>` section of `~/index.html`:

```html
<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Blazor App</title>
    <base href="/" />
    
    <!-- Syncfusion Theme -->
    <link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
</head>
```

Available themes: `bootstrap5.css`, `material3.css`, `tailwind.css`, `fluent2.css`, `highcontrast.css`

## Basic Component Usage

Add the Media Query component to a Razor page to detect and respond to screen size changes:

```razor
@using Syncfusion.Blazor

<SfMediaQuery @bind-ActiveBreakPoint="activeBreakpoint"></SfMediaQuery>

<h3>Current Breakpoint: @activeBreakpoint</h3>

@if (activeBreakpoint == "Small")
{
    <p>Mobile Device (≤768px)</p>
}
else if (activeBreakpoint == "Medium")
{
    <p>Tablet (768px - 1024px)</p>
}
else
{
    <p>Desktop (>1024px)</p>
}

@code {
    private string activeBreakpoint { get; set; }
}
```

The `@bind-ActiveBreakPoint` directive binds the current breakpoint to the `activeBreakpoint` variable. This variable updates automatically whenever the window resizes and crosses a breakpoint threshold.

**Test It:** Resize your browser window and watch the displayed message change as you cross breakpoint boundaries.
