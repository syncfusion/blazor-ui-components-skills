# ColorPicker - Getting Started

This guide covers the installation, basic setup, and initial configuration of the Syncfusion Blazor ColorPicker component.

## Installation

### NuGet Package

Install the `Syncfusion.Blazor.Inputs` package:

**Package Manager Console:**
```powershell
Install-Package Syncfusion.Blazor.Inputs
```

**NuGet Package Manager UI:**
1. Right-click on project → Manage NuGet Packages
2. Search for "Syncfusion.Blazor.Inputs"
3. Install the latest version

**.NET CLI:**
```bash
dotnet add package Syncfusion.Blazor.Inputs
```

## Service Registration

Register Syncfusion Blazor services in your application:

**Program.cs (.NET 6+):**
```csharp
using Syncfusion.Blazor;

var builder = WebApplication.CreateBuilder(args);
builder.Services.AddSyncfusionBlazor();
// ... other services

var app = builder.Build();
// ... middleware configuration
app.Run();
```

**Startup.cs (.NET 5 and earlier):**
```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSyncfusionBlazor();
}
```

## CSS Theme Configuration

Add Syncfusion theme CSS to your application:

**wwwroot/index.html (Blazor WebAssembly) or _Host.cshtml / _Layout.cshtml (Blazor Server):**
```html
<head>
    <!-- Material theme -->
    <link href="_content/Syncfusion.Blazor.Themes/material.css" rel="stylesheet" />
    
    <!-- Or choose another theme: -->
    <!-- <link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" /> -->
    <!-- <link href="_content/Syncfusion.Blazor.Themes/fluent.css" rel="stylesheet" /> -->
    <!-- <link href="_content/Syncfusion.Blazor.Themes/tailwind.css" rel="stylesheet" /> -->
</head>
```

## Namespace Import

Add the namespace in your component or `_Imports.razor`:

**_Imports.razor:**
```razor
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Inputs
```

## Basic ColorPicker Setup

### Minimal Example

```razor
@using Syncfusion.Blazor.Inputs

<SfColorPicker @bind-Value="@color"></SfColorPicker>

@code {
    private string color = "#008000";
}
```

This creates a basic color picker with the default Picker mode (gradient selector).

### With Label and Preview

```razor
@using Syncfusion.Blazor.Inputs

<div class="colorpicker-container">
    <label for="myColorPicker">Select Color:</label>
    <SfColorPicker @bind-Value="@selectedColor" 
                   ID="myColorPicker">
    </SfColorPicker>
    
    <div class="preview" style="background-color: @selectedColor; width: 100px; height: 50px; margin-top: 10px;">
        <p style="color: white; padding: 10px;">@selectedColor</p>
    </div>
</div>

@code {
    private string selectedColor = "#3498db";
}
```

## Value Binding

The ColorPicker value can be bound in multiple formats:

### Hex Format (Most Common)

```razor
<SfColorPicker @bind-Value="@hexColor"></SfColorPicker>

@code {
    private string hexColor = "#FF5733"; // Hex format
}
```

### RGBA Format (with opacity)

```razor
<SfColorPicker @bind-Value="@rgbaColor" 
               EnableOpacity="true">
</SfColorPicker>

@code {
    private string rgbaColor = "rgba(255, 87, 51, 0.5)"; // RGBA format
}
```

### HSVA Format

```razor
<SfColorPicker @bind-Value="@hsvaColor">
</SfColorPicker>

@code {
    private string hsvaColor = "hsva(9, 80%, 100%, 1)"; // HSVA format
}
```

## Mode Overview

The ColorPicker supports two selection modes:

### Picker Mode (Default - Gradient Selector)

```razor
<SfColorPicker @bind-Value="@color" 
               Mode="ColorPickerMode.Picker">
</SfColorPicker>

@code {
    private string color = "#008000";
}
```

**Features:**
- Full color spectrum gradient
- Hue, saturation, value controls
- Allows selection of any color
- Best for design tools and unrestricted selection

### Palette Mode (Predefined Colors Grid)

```razor
<SfColorPicker @bind-Value="@color" 
               Mode="ColorPickerMode.Palette">
</SfColorPicker>

@code {
    private string color = "#008000";
}
```

**Features:**
- Grid of predefined colors
- Quick selection from preset palette
- Configurable columns and color sets
- Best for limited color choices (brand colors, theme colors)

## Complete Getting Started Example

```razor
@page "/colorpicker-demo"
@using Syncfusion.Blazor.Inputs

<h3>ColorPicker Getting Started</h3>

<div class="example-section">
    <h4>1. Basic Picker Mode</h4>
    <SfColorPicker @bind-Value="@pickerColor" 
                   Mode="ColorPickerMode.Picker">
    </SfColorPicker>
    <p>Selected: @pickerColor</p>
</div>

<div class="example-section">
    <h4>2. Palette Mode</h4>
    <SfColorPicker @bind-Value="@paletteColor" 
                   Mode="ColorPickerMode.Palette">
    </SfColorPicker>
    <p>Selected: @paletteColor</p>
</div>

<div class="example-section">
    <h4>3. With Opacity</h4>
    <SfColorPicker @bind-Value="@opacityColor" 
                   EnableOpacity="true">
    </SfColorPicker>
    <p>Selected: @opacityColor</p>
    <div style="background-color: @opacityColor; width: 200px; height: 50px; border: 1px solid #ccc;">
        Preview with opacity
    </div>
</div>

@code {
    private string pickerColor = "#e74c3c";
    private string paletteColor = "#3498db";
    private string opacityColor = "rgba(46, 204, 113, 0.5)";
}

<style>
    .example-section {
        margin-bottom: 30px;
        padding: 20px;
        border: 1px solid #ddd;
        border-radius: 5px;
    }
</style>
```

## Initial Value Configuration

### Setting Initial Color

```razor
<SfColorPicker @bind-Value="@brandColor"></SfColorPicker>

@code {
    private string brandColor = "#e74c3c"; // Set initial value
    
    protected override void OnInitialized()
    {
        // Can also set dynamically on initialization
        brandColor = GetBrandColorFromSettings();
    }
    
    private string GetBrandColorFromSettings()
    {
        // Retrieve from settings, database, etc.
        return "#3498db";
    }
}
```

### Default Value from User Preferences

```razor
<SfColorPicker @bind-Value="@userThemeColor"></SfColorPicker>

@code {
    private string userThemeColor;
    
    protected override async Task OnInitializedAsync()
    {
        // Load from user preferences
        userThemeColor = await LoadUserPreferenceAsync("themeColor") ?? "#008000";
    }
    
    private async Task<string> LoadUserPreferenceAsync(string key)
    {
        // Simulate loading from local storage or API
        return "#9b59b6";
    }
}
```

## Next Steps

- **Modes & Configuration**: Learn about Picker vs Palette modes, inline display, and mode switching → [colorpicker-modes-configuration.md](colorpicker-modes-configuration.md)
- **Preset Colors**: Configure custom color palettes and recent colors → [colorpicker-presets-colors.md](colorpicker-presets-colors.md)
- **Opacity**: Enable alpha channel for transparency → [colorpicker-opacity-values.md](colorpicker-opacity-values.md)
- **Events**: Handle value changes and user interactions → [colorpicker-events-binding.md](colorpicker-events-binding.md)
- **Customization**: Styling, accessibility, and advanced customization → [colorpicker-customization-accessibility.md](colorpicker-customization-accessibility.md)
