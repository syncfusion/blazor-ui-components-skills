# Getting Started with Blazor Splitter

## Table of Contents
- [Installation](#installation)
- [Project Setup](#project-setup)
- [Basic Implementation](#basic-implementation)
- [Blazor Web App Setup](#blazor-web-app-setup)
- [First Render](#first-render)

## Installation

### NuGet Package Installation

The Splitter component requires two NuGet packages:

**Visual Studio Package Manager:**
```
Install-Package Syncfusion.Blazor.Layouts
Install-Package Syncfusion.Blazor.Themes
```

**Visual Studio Code / .NET CLI:**
```bash
dotnet add package Syncfusion.Blazor.Layouts
dotnet add package Syncfusion.Blazor.Themes
dotnet restore
```

Both packages are required. The Layouts package contains the Splitter component, while the Themes package provides styling.

## Project Setup

### Step 1: Add Namespace Imports

Edit the `~/_Imports.razor` file and add the Syncfusion namespaces:

```csharp
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Layouts
```

These imports must be in the root imports file to be available throughout your application.

### Step 2: Register Syncfusion Blazor Service

Update `Program.cs` to register the Syncfusion service:

```csharp
using Syncfusion.Blazor;

var builder = WebAssemblyHostBuilder.CreateDefault(args);
builder.RootComponents.Add<App>("#app");
builder.RootComponents.Add<HeadOutlet>("head::after");

builder.Services.AddScoped(sp => new HttpClient { BaseAddress = new Uri(builder.HostEnvironment.BaseAddress) });

// Register Syncfusion Blazor Service
builder.Services.AddSyncfusionBlazor();

await builder.Build().RunAsync();
```

### Step 3: Add Stylesheet and Script Resources

Add the theme stylesheet and script references to `~/index.html` in the `<head>` section:

```html
<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Splitter App</title>
    <base href="/" />
    
    <!-- Syncfusion Theme -->
    <link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
    <link href="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js" type="text/javascript"></link>
</head>
```

**Available themes:** bootstrap5, material3, fluent2, tailwind

Choose the theme that matches your design system.

## Basic Implementation

### Horizontal Splitter (Default)

The most basic splitter divides the container horizontally (left-to-right):

```csharp
@using Syncfusion.Blazor.Layouts

<div>Horizontal Splitter</div>

<SfSplitter Height="240px" Width="100%">
    <SplitterPanes>
        <SplitterPane>
            <ContentTemplate>
                <div class="centered-content">
                    Left Pane
                </div>
            </ContentTemplate>
        </SplitterPane>
        <SplitterPane>
            <ContentTemplate>
                <div class="centered-content">
                    Middle Pane
                </div>
            </ContentTemplate>
        </SplitterPane>
        <SplitterPane>
            <ContentTemplate>
                <div class="centered-content">
                    Right Pane
                </div>
            </ContentTemplate>
        </SplitterPane>
    </SplitterPanes>
</SfSplitter>

<style>
    .centered-content {
        display: flex;
        justify-content: center;
        align-items: center;
        height: 100%;
    }
</style>
```

**Output:** Three equally-sized horizontal panes with auto-sizing.

### Vertical Splitter

For vertical (top-to-bottom) split, use `Orientation="Orientation.Vertical"`:

```csharp
@using Syncfusion.Blazor.Layouts

<div>Vertical Splitter</div>

<SfSplitter Height="305px" Width="600px" Orientation="Orientation.Vertical">
    <SplitterPanes>
        <SplitterPane Size="100px">
            <ContentTemplate>
                <div class="centered-content">
                    Top Pane
                </div>
            </ContentTemplate>
        </SplitterPane>
        <SplitterPane Size="100px">
            <ContentTemplate>
                <div class="centered-content">
                    Middle Pane
                </div>
            </ContentTemplate>
        </SplitterPane>
        <SplitterPane Size="100px">
            <ContentTemplate>
                <div class="centered-content">
                    Bottom Pane
                </div>
            </ContentTemplate>
        </SplitterPane>
    </SplitterPanes>
</SfSplitter>

<style>
    .centered-content {
        display: flex;
        justify-content: center;
        align-items: center;
        height: 100%;
    }
</style>
```

## Blazor Web App Setup

For Blazor Web App (with server + client components), follow these additional steps:

### Step 1: Install Packages in Client Project

If using WebAssembly or Auto render modes, install packages in the **client project** folder:

```bash
cd BlazorWebApp.Client
dotnet add package Syncfusion.Blazor.Layouts
dotnet add package Syncfusion.Blazor.Themes
```

### Step 2: Add Imports in Client ~/_Imports.razor

Add namespaces to the client project's `~/_Imports.razor`:

```csharp
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Layouts
```

### Step 3: Register Service in Program.cs

Register service in **both** server and client `Program.cs` if using WebAssembly/Auto render modes:

```csharp
// Client Program.cs
builder.Services.AddSyncfusionBlazor();

// Server Program.cs (if WebAssembly or Auto mode)
builder.Services.AddSyncfusionBlazor();
```

### Step 4: Add Resources to App.razor

Update `~/Components/App.razor` with stylesheet and script:

```html
<head>
    <link href="_content/Syncfusion.Blazor.Themes/fluent2.css" rel="stylesheet" />
</head>

<body>
    ...
    <script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js"></script>
</body>
```

### Step 5: Define Render Mode (Per-Page Components)

If Interactivity Location is set to "Per page/component", add render mode to your component:

```csharp
@rendermode InteractiveAuto

@using Syncfusion.Blazor.Layouts

<SfSplitter Height="240px" Width="100%">
    <!-- Panes here -->
</SfSplitter>
```

## First Render

### Minimal Complete Example

Create a new Razor page `Splitter.razor` in `~/Pages/`:

```csharp
@page "/splitter"
@using Syncfusion.Blazor.Layouts

<PageTitle>Splitter Component</PageTitle>

<h1>Syncfusion Blazor Splitter</h1>

<SfSplitter Height="300px" Width="100%">
    <SplitterPanes>
        <SplitterPane Size="300px">
            <ContentTemplate>
                <div style="padding: 20px;">
                    <h3>Left Panel</h3>
                    <p>This is the left pane content.</p>
                </div>
            </ContentTemplate>
        </SplitterPane>
        <SplitterPane>
            <ContentTemplate>
                <div style="padding: 20px;">
                    <h3>Right Panel</h3>
                    <p>This is the right pane content. It auto-sizes to fill remaining space.</p>
                </div>
            </ContentTemplate>
        </SplitterPane>
    </SplitterPanes>
</SfSplitter>
```

Navigate to `/splitter` to see the rendered component.

### Key Takeaways

1. **Two packages required**: Syncfusion.Blazor.Layouts + Syncfusion.Blazor.Themes
2. **Add to `_Imports.razor`**: Syncfusion and Syncfusion.Blazor.Layouts namespaces
3. **Register service**: `builder.Services.AddSyncfusionBlazor()` in Program.cs
4. **Include resources**: Theme CSS and script in index.html or App.razor
5. **Render mode** (Web App only): Add `@rendermode` if using per-component interactivity

## Troubleshooting

**Issue**: Component doesn't render, appears blank
- Check that both NuGet packages are installed
- Verify stylesheet and script are loaded (check browser DevTools)
- Confirm namespace imports in _Imports.razor

**Issue**: Styling looks wrong or default theme not applied
- Ensure correct theme name in stylesheet link (bootstrap5, material3, fluent2, tailwind)
- Verify CSS path is `_content/Syncfusion.Blazor.Themes/[theme].css`

**Issue**: "SfSplitter not found" compilation error
- Add `@using Syncfusion.Blazor.Layouts` to _Imports.razor
- Check project builds without errors

---

**Next:** Read [Split Panes and Layouts](split-panes.md) to understand horizontal/vertical orientation and pane sizing.
