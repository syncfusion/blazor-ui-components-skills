# Getting Started with Dashboard Layout

## Table of Contents
- [WebAssembly App Setup](#webassembly-app-setup)
- [Server App Setup](#server-app-setup)
- [Web App Setup](#web-app-setup)
- [Basic Component Usage](#basic-component-usage)
- [Panels with Headers and Content](#panels-with-headers-and-content)

## WebAssembly App Setup

### Prerequisites
- System requirements for Blazor components

### Create Blazor WebAssembly App

You can create a Blazor WebAssembly App using:
- Microsoft Templates in Visual Studio
- Syncfusion Blazor Extension
- Visual Studio Code with templates
- .NET CLI with command: `dotnet new blazorwasm -o BlazorApp`

### Install NuGet Packages

Install the following NuGet packages using NuGet Package Manager or dotnet CLI:

```
Syncfusion.Blazor.Layouts
Syncfusion.Blazor.Themes
```

**Via Package Manager Console:**
```
Install-Package Syncfusion.Blazor.Layouts
Install-Package Syncfusion.Blazor.Themes
```

**Via dotnet CLI:**
```
dotnet add package Syncfusion.Blazor.Layouts
dotnet add package Syncfusion.Blazor.Themes
dotnet restore
```

### Add Import Namespaces

Open `~/_Imports.razor` file and add:

```csharp
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Layouts
```

### Register Syncfusion Service

In `Program.cs`, add:

```csharp
using Syncfusion.Blazor;
...
builder.Services.AddSyncfusionBlazor();
...
```

### Add StyleSheet and Script Resources

In `~/Components/App.razor` or your main layout file, add stylesheet and script:

```html
<link href="_content/Syncfusion.Blazor.Themes/fluent2.css" rel="stylesheet" />
...
<script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js" type="text/javascript"></script>
```

## Server App Setup

### Prerequisites
- System requirements for Blazor components

### Create Blazor Server App

Create a **Blazor Server App** (Blazor Web App with Server rendering) using:
- Microsoft Templates via Blazor Web App option
- Syncfusion Blazor Extension
- .NET CLI: `dotnet new blazor -o BlazorApp -int Server`

### Install NuGet Packages

Same as WebAssembly:
```
Install-Package Syncfusion.Blazor.Layouts
Install-Package Syncfusion.Blazor.Themes
```

### Add Import Namespaces

In `~/_Imports.razor`, add:

```csharp
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Layouts
```

### Register Syncfusion Service

In `Program.cs`, add:

```csharp
using Syncfusion.Blazor;
...
builder.Services.AddSyncfusionBlazor();
...
```

### Add StyleSheet and Script Resources

In `~/Components/App.razor`, add:

```html
<link href="_content/Syncfusion.Blazor.Themes/fluent2.css" rel="stylesheet" />
...
<script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js" type="text/javascript"></script>
```

### Add Dashboard Layout Component

In your razor page (e.g., `~/Components/Pages/Home.razor`), if using per-page interactivity, define render mode:

```razor
@rendermode InteractiveServer
```

Then add the component:

```razor
@using Syncfusion.Blazor.Layouts

<SfDashboardLayout>
    <DashboardLayoutPanels>
        <DashboardLayoutPanel>
        </DashboardLayoutPanel>
    </DashboardLayoutPanels>
</SfDashboardLayout>
```

Press **Ctrl+F5** (Windows) or **⌘+F5** (macOS) to launch.

## Web App Setup

### Prerequisites
- System requirements for Blazor components

### Create Blazor Web App

Create a **Blazor Web App** using:
- Microsoft Templates with appropriate interactive render mode
- Syncfusion Blazor Extension
- .NET CLI: `dotnet new blazor -o BlazorWebApp -int Auto`

For WebAssembly or Auto mode: Navigate to client project before installing packages.

### Install NuGet Packages

```
Install-Package Syncfusion.Blazor.Layouts
Install-Package Syncfusion.Blazor.Themes
```

If using WebAssembly or Auto mode, install in the **client project**.

### Add Import Namespaces

In `~/_Imports.razor` (client project for WebAssembly/Auto), add:

```csharp
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Layouts
```

### Register Syncfusion Service

In `Program.cs` of Blazor Web App (server project):

```csharp
using Syncfusion.Blazor;
...
builder.Services.AddSyncfusionBlazor();
...
```

If using WebAssembly or Auto mode, register in **both** server and client `Program.cs` files.

### Add StyleSheet and Script Resources

In `~/Components/App.razor`, add:

```html
<link href="_content/Syncfusion.Blazor.Themes/fluent2.css" rel="stylesheet" />
...
<script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js" type="text/javascript"></script>
```

### Add Dashboard Layout Component

In your razor page, define render mode if per-page interactivity:

```razor
@rendermode InteractiveAuto
```

Or appropriate mode: `InteractiveServer`, `InteractiveWebAssembly`, `InteractiveAuto`.

Then add component:

```razor
@using Syncfusion.Blazor.Layouts

<SfDashboardLayout>
    <DashboardLayoutPanels>
        <DashboardLayoutPanel></DashboardLayoutPanel>
    </DashboardLayoutPanels>
</SfDashboardLayout>
```

## Basic Component Usage

### Minimal Dashboard

```csharp
@using Syncfusion.Blazor.Layouts

<SfDashboardLayout>
    <DashboardLayoutPanels>
        <DashboardLayoutPanel>
            <ContentTemplate><div>Panel 1</div></ContentTemplate>
        </DashboardLayoutPanel>
    </DashboardLayoutPanels>
</SfDashboardLayout>
```

### Dashboard with Grid Configuration

```csharp
@using Syncfusion.Blazor.Layouts

<SfDashboardLayout CellSpacing="@(new double[]{10, 10})" Columns="5">
    <DashboardLayoutPanels>
        <DashboardLayoutPanel>
            <ContentTemplate><div>Panel 1</div></ContentTemplate>
        </DashboardLayoutPanel>
        <DashboardLayoutPanel Column=1>
            <ContentTemplate><div>Panel 2</div></ContentTemplate>
        </DashboardLayoutPanel>
        <DashboardLayoutPanel Column=2>
            <ContentTemplate><div>Panel 3</div></ContentTemplate>
        </DashboardLayoutPanel>
    </DashboardLayoutPanels>
</SfDashboardLayout>
```

## Panels with Headers and Content

### Using HeaderTemplate and ContentTemplate

The **HeaderTemplate** displays a title or summary at the top of each panel. The **ContentTemplate** holds the main panel content.

```csharp
@using Syncfusion.Blazor.Layouts

<SfDashboardLayout CellSpacing="@(new double[]{10, 10})" Columns="5">
    <DashboardLayoutPanels>
        <DashboardLayoutPanel>
            <HeaderTemplate><div>Panel 1</div></HeaderTemplate>
            <ContentTemplate><div>Panel Content</div></ContentTemplate>
        </DashboardLayoutPanel>
        <DashboardLayoutPanel SizeX=2 SizeY=2 Column=1>
            <HeaderTemplate><div>Panel 2</div></HeaderTemplate>
            <ContentTemplate><div>Panel Content</div></ContentTemplate>
        </DashboardLayoutPanel>
        <DashboardLayoutPanel SizeY=2 Column=3>
            <HeaderTemplate><div>Panel 3</div></HeaderTemplate>
            <ContentTemplate><div>Panel Content</div></ContentTemplate>
        </DashboardLayoutPanel>
        <DashboardLayoutPanel Row=1>
            <HeaderTemplate><div>Panel 4</div></HeaderTemplate>
            <ContentTemplate><div>Panel Content</div></ContentTemplate>
        </DashboardLayoutPanel>
    </DashboardLayoutPanels>
</SfDashboardLayout>

<style>
    .e-panel-header {
        background-color: rgba(0, 0, 0, .1);
        text-align: center;
    }
    .e-panel-content {
        text-align: center;
        margin-top: 10px;
    }
</style>
```

### Hosting Components in Panels

Beyond simple content, panels can host complex Syncfusion Blazor components:

```csharp
@using Syncfusion.Blazor.Layouts
@using Syncfusion.Blazor.Charts  // Example: for charts

<SfDashboardLayout CellSpacing="@(new double[]{10, 10})" Columns="5">
    <DashboardLayoutPanels>
        <DashboardLayoutPanel SizeX=2 SizeY=2>
            <HeaderTemplate><div>Chart</div></HeaderTemplate>
            <ContentTemplate>
                <SfChart>
                    <!-- Chart configuration -->
                </SfChart>
            </ContentTemplate>
        </DashboardLayoutPanel>
    </DashboardLayoutPanels>
</SfDashboardLayout>
```

Replace `SfChart` with any other Syncfusion component like `SfGrid`, `SfAccumulationChart`, `SfGauge`, etc.

### Styling Headers and Content

Use CSS to customize appearance:

```css
.e-panel-header {
    background-color: #334099;
    color: white;
    padding: 10px;
    font-weight: bold;
}

.e-panel-content {
    background-color: #f5f5f5;
    padding: 20px;
}
```
