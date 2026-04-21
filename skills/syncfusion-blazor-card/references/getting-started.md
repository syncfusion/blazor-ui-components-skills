# Getting Started with Syncfusion Blazor Card

Learn how to install the Syncfusion Blazor Card component and create your first card in different Blazor project types.

## Table of Contents

- [Prerequisites](#prerequisites)
- [Project Setup - Choose Your Blazor Type](#project-setup---choose-your-blazor-type)
  - [Blazor WebAssembly App](#blazor-webassembly-app)
  - [Blazor Web App](#blazor-web-app)
  - [Blazor Server App](#blazor-server-app)
- [Install NuGet Packages](#install-nuget-packages)
- [Configure Imports](#configure-imports)
- [Register Syncfusion Service](#register-syncfusion-service)
- [Add Theme](#add-theme)
- [Create Your First Card](#create-your-first-card)
- [Run Your Application](#run-your-application)
- [Verify Installation](#verify-installation)
- [Next Steps](#next-steps)
- [Troubleshooting](#troubleshooting)

## Prerequisites

Before getting started, ensure your system meets the following requirements:
- [System requirements for Blazor components](url)
- Visual Studio, Visual Studio Code, or .NET CLI installed
- .NET SDK (latest version recommended)

## Project Setup - Choose Your Blazor Type

### Blazor WebAssembly App

#### Create a Blazor WASM App

Using Visual Studio:
1. Create a new **Blazor WebAssembly App** project
2. Select appropriate target framework

Using .NET CLI:
```bash
dotnet new blazorwasm -o BlazorApp
cd BlazorApp
```

### Blazor Web App

#### Create a Blazor Web App

Using Visual Studio:
1. Create a new **Blazor Web App** project
2. Configure interactive render mode (Auto, Server, or WebAssembly)

Using .NET CLI:
```bash
dotnet new blazor -o BlazorWebApp -int Auto
cd BlazorWebApp
cd BlazorWebApp.Client
```

### Blazor Server App

#### Create a Blazor Server App

Using Visual Studio:
1. Use the **Blazor Web App** template
2. Select Server App configuration

Using .NET CLI:
```bash
dotnet new blazor -o BlazorApp -int Server
cd BlazorApp
```

## Install NuGet Packages

Install the required Syncfusion packages using NuGet Package Manager or CLI.

### Using Package Manager Console

```powershell
Install-Package Syncfusion.Blazor.Cards -Version 24.2.0
Install-Package Syncfusion.Blazor.Themes -Version 24.2.0
```

### Using dotnet CLI

```bash
dotnet add package Syncfusion.Blazor.Cards
dotnet add package Syncfusion.Blazor.Themes
```

## Configure Imports

Add namespaces to your **_Imports.razor** file:

```csharp
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Cards
```

## Register Syncfusion Service

Register the Syncfusion Blazor service in your application startup.

### For Blazor WebAssembly/Web App

In **Program.cs**:
```csharp
using Syncfusion.Blazor;

builder.Services.AddSyncfusionBlazor();
```

### For Blazor Server App

In **Program.cs**:
```csharp
using Syncfusion.Blazor;

builder.Services.AddSyncfusionBlazor();
```

## Add Theme

Include a Syncfusion theme in your **index.html** (WebAssembly/Web App) or **Pages/_Host.cshtml** (Server App):

```html
<link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
```

Available themes: `bootstrap5`, `material`, `fabric`, `highcontrast`.

## Create Your First Card

In a Blazor component (e.g., **Index.razor** or **Card.razor**):

```razor
@page "/"

<SfCard>
    <CardHeader Title="Welcome to Syncfusion Card"></CardHeader>
    <CardContent Content="This is your first Syncfusion Blazor Card component!"></CardContent>
</SfCard>
```

## Run Your Application

Build and run your application:

```bash
dotnet build
dotnet run
```

Navigate to `http://localhost:5000` (or your configured port) to see your card rendered.

## Verify Installation

You should see:
- A card container with header text "Welcome to Syncfusion Card"
- Content text displayed below the header
- Default styling applied from the theme

## Next Steps

- [Building Card Structure](building-card-structure.md) - Add images, multiple sections, and dividers
- [Card Layouts and Orientation](card-layouts-and-orientation.md) - Create horizontal and stacked layouts
- [Action Buttons and Footers](action-buttons-and-footers.md) - Add interactive buttons
- [Styling and Customization](styling-and-customization.md) - Customize appearance with CSS

## Troubleshooting

### Card not rendering
- Verify Syncfusion.Blazor.Cards package is installed
- Check that namespaces are imported in _Imports.razor
- Ensure Syncfusion service is registered in Program.cs
- Confirm theme CSS is included

### Styling not applied
- Verify theme CSS link is added to HTML
- Check browser console for CSS loading errors
- Clear browser cache and rebuild solution

### Components not recognized
- Confirm @using Syncfusion.Blazor.Cards is in _Imports.razor
- Restart the development server
- Clean and rebuild the solution
