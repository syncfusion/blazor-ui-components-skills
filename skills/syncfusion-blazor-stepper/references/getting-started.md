# Getting Started with Blazor Stepper

## Table of Contents

- [Installation and Setup](#installation-and-setup)
- [Blazor Web App Setup](#blazor-web-app-setup)
- [Namespaces](#namespaces)
- [Service Registration](#service-registration)
- [Theme & Script References](#theme--script-references)
- [Basic Examples](#basic-examples)
- [CSS Themes](#css-themes)
- [Troubleshooting](#troubleshooting)

## Installation and Setup

### Prerequisites
- Visual Studio 2022 or Visual Studio Code
- .NET SDK (latest version)
- Syncfusion Blazor license or trial

### Step 1: Create a Blazor WebAssembly App

Create a new Blazor WebAssembly application using Visual Studio templates or the .NET CLI:

```bash
dotnet new blazorwasm -o BlazorApp
cd BlazorApp
```

### Step 2: Install NuGet Packages

Install the required Syncfusion NuGet packages:

**Using Package Manager Console:**
```
Install-Package Syncfusion.Blazor.Navigations
Install-Package Syncfusion.Blazor.Themes
```

**Using .NET CLI:**
```bash
dotnet add package Syncfusion.Blazor.Navigations
dotnet add package Syncfusion.Blazor.Themes
dotnet restore
```

### Step 3: Add Imports

Open `_Imports.razor` and add the required namespaces:

```csharp
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Navigations
```

### Step 4: Register Syncfusion Service

In `Program.cs`, register the Syncfusion Blazor service:

```csharp
using Microsoft.AspNetCore.Components.Web;
using Microsoft.AspNetCore.Components.WebAssembly.Hosting;
using Syncfusion.Blazor;

var builder = WebAssemblyHostBuilder.CreateDefault(args);
builder.RootComponents.Add<App>("#app");
builder.RootComponents.Add<HeadOutlet>("head::after");

builder.Services.AddScoped(sp => new HttpClient { BaseAddress = new Uri(builder.HostEnvironment.BaseAddress) });

// Register Syncfusion Blazor Service
builder.Services.AddSyncfusionBlazor();
await builder.Build().RunAsync();
```

### Step 5: Add Theme and Script References

In `index.html`, add the stylesheet and script references within the `<head>` section:

```html
<head>
    <!-- ... other head elements ... -->
    <link href="_content/Syncfusion.Blazor.Themes/material3.css" rel="stylesheet" />
    <script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js" type="text/javascript"></script>
</head>
```

## Blazor Web App Setup

For modern **Blazor Web App** projects (with Auto, WebAssembly, or Server render modes):

### Create Blazor Web App

**Using Visual Studio:**
1. Create a **Blazor Web App** project
2. Configure **Interactive Render Mode** (Auto, WebAssembly, or Server)
3. Select location (Server or Client)

**Using .NET CLI:**
```bash
dotnet new blazor -o BlazorWebApp -int Auto
cd BlazorWebApp
cd BlazorWebApp.Client
```

### Install Packages (Web App)

Install packages in the **client project** (for WebAssembly/Auto modes):

```powershell
Install-Package Syncfusion.Blazor.Navigations
Install-Package Syncfusion.Blazor.Themes
```

### Register Services (Web App)

**Server-side** (Program.cs):
```csharp
using Syncfusion.Blazor;

builder.Services.AddSyncfusionBlazor();
builder.Services.AddRazorComponents()
    .AddInteractiveWebAssemblyComponents();
```

**Client-side** (_Program.cs in client project):
```csharp
using Syncfusion.Blazor;

var builder = WebAssemblyHostBuilder.CreateDefault(args);
builder.Services.AddSyncfusionBlazor();
await builder.Build().RunAsync();
```

### Enable Interactive Rendering

Add `@rendermode InteractiveAuto` in your Razor page:
```csharp
@rendermode InteractiveAuto
@page "/"

<MyStepper />
```

## Namespaces

Add these using statements in `_Imports.razor`:
```csharp
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Navigations
```

## Service Registration

See Installation and Setup or Blazor Web App Setup sections above for service registration details.

## Theme & Script References

Add to `index.html` `<head>` section:
```html
<link href="_content/Syncfusion.Blazor.Themes/material3.css" rel="stylesheet" />
<script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js"></script>
```

## Basic Examples

### Minimal Example

```csharp
@using Syncfusion.Blazor.Navigations

<SfStepper>
    <StepperSteps>
        <StepperStep></StepperStep>
        <StepperStep></StepperStep>
        <StepperStep></StepperStep>
        <StepperStep></StepperStep>
    </StepperSteps>
</SfStepper>
```

### Example with Icons and Labels

```csharp
@using Syncfusion.Blazor.Navigations

<SfStepper>
    <StepperSteps>
        <StepperStep Label="Cart" IconCss="sf-icon-cart"></StepperStep>
        <StepperStep Label="Address" IconCss="sf-icon-user"></StepperStep>
        <StepperStep Label="Delivery" IconCss="sf-icon-transport"></StepperStep>
        <StepperStep Label="Payment" IconCss="sf-icon-payment"></StepperStep>
        <StepperStep Label="Ordered" IconCss="sf-icon-success"></StepperStep>
    </StepperSteps>
</SfStepper>

<style>
    @@font-face {
        font-family: 'Default';
        src: url(data:application/x-font-ttf;charset=utf-8;base64,...) format('truetype');
        font-weight: normal;
        font-style: normal;
    }

    [class^="sf-icon-"], [class*=" sf-icon-"] {
        font-family: 'Default' !important;
        speak: none;
        font-style: normal;
        font-weight: normal;
        font-variant: normal;
        text-transform: none;
        line-height: inherit;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
    }

    .sf-icon-cart:before { content: "\e710"; }
    .sf-icon-user:before { content: "\e708"; }
    .sf-icon-transport:before { content: "\e702"; }
    .sf-icon-payment:before { content: "\e706"; }
    .sf-icon-success:before { content: "\e715"; }
</style>
```

### Example with Text Content

```csharp
@using Syncfusion.Blazor.Navigations

<SfStepper StepType="StepperType.Indicator">
    <StepperSteps>
        <StepperStep Text="A"></StepperStep>
        <StepperStep Text="B"></StepperStep>
        <StepperStep Text="C"></StepperStep>
        <StepperStep Text="D"></StepperStep>
    </StepperSteps>
</SfStepper>

<SfStepper ID="labelStepper">
    <StepperSteps>
        <StepperStep Label="Cart"></StepperStep>
        <StepperStep Label="Delivery"></StepperStep>
        <StepperStep Label="Payment"></StepperStep>
        <StepperStep Label="Confirmation"></StepperStep>
    </StepperSteps>
</SfStepper>

<style>
    #labelStepper {
        margin-top: 50px;
    }
</style>
```

## CSS Themes

Syncfusion provides multiple theme options. Change the theme by replacing the stylesheet in `index.html`:

**Available Themes:**
- `material3.css` - Material Design 3 (default)
- `bootstrap5.css` - Bootstrap 5 theme
- `fluent.css` - Microsoft Fluent theme
- `tailwind.css` - Tailwind CSS theme
- `fabric.css` - Office Fabric theme

Example with Bootstrap 5 theme:

```html
<link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
```

## First Render

After implementing the Stepper component, press `Ctrl+F5` (Windows) or `Cmd+F5` (macOS) to launch the application. The Stepper will render horizontally by default with all steps visible and the first step active.

## Troubleshooting

**Issue**: Component not rendering
- **Solution**: Verify that the theme stylesheet is correctly referenced in `index.html`

**Issue**: Icons not displaying
- **Solution**: Ensure the font-face CSS is included and the IconCss property references valid icon classes

**Issue**: Script errors in console
- **Solution**: Check that the Syncfusion script reference in `index.html` is correct and the file exists

## Next Steps

- Explore step types and label positioning in Step Types and Configuration
- Add event handlers for step progression
- Implement custom templates for advanced layouts
- Enable validation for form-based workflows
