## Table of Contents

- [Installation and Setup](#installation-and-setup)
   - [Step 1: Install NuGet Package](#step-1-install-nuget-package)
   - [Step 2: Register in Program.cs](#step-2-register-in-programcs)
   - [Step 3: Add CSS Theme Reference](#step-3-add-css-theme-reference)
- [Basic Map Component](#basic-map-component)
   - [Minimal Map Setup](#minimal-map-setup)
   - [Map with Default Center Position](#map-with-default-center-position)
   - [Map with Fixed Size](#map-with-fixed-size)
- [First Interactive Map Example](#first-interactive-map-example)
- [Understanding Map Layers](#understanding-map-layers)
   - [What is a Layer?](#what-is-a-layer)
- [CSS and Theme Configuration](#css-and-theme-configuration)
   - [Import via CSS File](#import-via-css-file)
   - [Dynamic Theme Switching](#dynamic-theme-switching)
- [Container Requirements](#container-requirements)
- [Troubleshooting Setup Issues](#troubleshooting-setup-issues)
   - [Map appears blank or doesn't load](#map-appears-blank-or-doesnt-load)
   - ["SfMaps is not defined" or compilation error](#sfmaps-is-not-defined-or-compilation-error)
   - [Map renders but tiles don't load](#map-renders-but-tiles-dont-load)
- [Key Properties for Basic Setup](#key-properties-for-basic-setup)
- [SfMaps Component API](#sfmaps-component-api)
   - [Essential Properties](#essential-properties)
   - [Essential Methods](#essential-methods)
- [Next Steps](#next-steps)

# Getting Started with Syncfusion Maps

## Installation and Setup

### Step 1: Install NuGet Package

Install the Syncfusion.Blazor package via NuGet Package Manager:

```bash
dotnet add package Syncfusion.Blazor
```

Or using Package Manager Console:
```
Install-Package Syncfusion.Blazor
```

### Step 2: Register in Program.cs

For Blazor Web App (Server or Auto), add the Syncfusion service in `Program.cs`:

```csharp
using Syncfusion.Blazor;

var builder = WebApplication.CreateBuilder(args);

// Add services to the container
builder.Services.AddRazorComponents()
    .AddInteractiveServerComponents();

builder.Services.AddSyncfusionBlazor();

var app = builder.Build();

app.Run();
```

For older Blazor WASM apps, register in `Main.cs` or `Program.cs`:
```csharp
builder.Services.AddSyncfusionBlazor();
```

### Step 3: Add CSS Theme Reference

In your `App.razor` or main layout file, add the Syncfusion CSS reference:

```html
<link href="_content/Syncfusion.Blazor/styles/bootstrap5.css" rel="stylesheet" />
```

Common theme options:
- `bootstrap5.css` - Bootstrap 5 theme (default)
- `material.css` - Material Design theme
- `fabric.css` - Fabric theme
- `tailwind.css` - Tailwind CSS theme

Choose the theme that matches your application's design system.

## Basic Map Component

### Minimal Map Setup

Create a new component or page with a basic map:

```csharp
@page "/maps"
@using Syncfusion.Blazor.Maps

<SfMaps>
    <MapsLayers>
        <MapsLayer UrlTemplate="https://tile.openstreetmap.org/level/tileX/tileY.png" TValue="string">
        </MapsLayer>
    </MapsLayers>
</SfMaps>
```

This creates an interactive map using OpenStreetMap tiles. Users can immediately zoom and pan.

### Map with Default Center Position

Control the initial map view:

```csharp
<SfMaps >
<MapsZoomSettings ZoomFactor="4"></MapsZoomSettings>
    <MapsLayers>
        <MapsLayer UrlTemplate="https://tile.openstreetmap.org/level/tileX/tileY.png" TValue="string">
        </MapsLayer>
    </MapsLayers>
</SfMaps>

@code {
    private int ZoomLevel = 4;
}
```

**Properties:**
- `CenterPosition.Latitude`: Initial latitude (e.g., 37.368)
- `CenterPosition.Longitude`: Initial longitude (e.g., -122.095)
- `ZoomLevel`: Initial zoom level (1-20, default is 1)

### Map with Fixed Size

Set explicit dimensions for the map container:

```csharp
<div style="width: 100%; height: 600px;">
    <SfMaps>
        <MapsLayers>
            <MapsLayer UrlTemplate="https://tile.openstreetmap.org/level/tileX/tileY.png" TValue="string">
            </MapsLayer>
        </MapsLayers>
    </SfMaps>
</div>
```

Without explicit sizing, maps default to 400px height. Always wrap in a container with defined height.

## First Interactive Map Example

Complete working example with markers and basic interaction:

```csharp
@page "/maps-demo"
@using Syncfusion.Blazor.Maps

<h2>My First Map</h2>

<div style="width: 100%; height: 500px;">
    <SfMaps >
    <MapsZoomSettings ZoomFactor="4"></MapsZoomSettings>
        <MapsLayers>
            <MapsLayer UrlTemplate="https://tile.openstreetmap.org/level/tileX/tileY.png" TValue="string">
                <MapsMarkerSettings>
                    <MapsMarker Latitude="37.368" Longitude="-122.095" 
                        Shape="MarkerType.Circle" Width="15" Height="15">
                    </MapsMarker>
                    <MapsMarker Latitude="40.726" Longitude="-73.986" 
                        Shape="MarkerType.Circle" Width="15" Height="15">
                    </MapsMarker>
                </MapsMarkerSettings>
            </MapsLayer>
        </MapsLayers>
    </SfMaps>
</div>

@code {
    private MapsCenterPosition CenterLatLng = new MapsCenterPosition { Latitude = 39.0, Longitude = -98.0 };
}
```

This creates a map centered on the continental US with two markers. Users can click, zoom, and drag to navigate.

## Understanding Map Layers

### What is a Layer?

A layer is a collection of visual elements (tiles, shapes, markers) displayed on the map at a specific level. Maps can have multiple layers:

```csharp
<SfMaps>
    <MapsLayers>
        <MapsLayer UrlTemplate="https://tile.openstreetmap.org/level/tileX/tileY.png" TValue="string">
            <!-- Base tile layer -->
        </MapsLayer>
        <MapsLayer TValue="string">
            <!-- Shape data layer -->
        </MapsLayer>
        <MapsLayer TValue="string">
            <!-- Marker overlay layer -->
        </MapsLayer>
    </MapsLayers>
</SfMaps>
```

Each layer renders independently and can be toggled on/off.

## CSS and Theme Configuration

### Import via CSS File

In your `index.html` or layout, add the Syncfusion theme link:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="utf-8" />
    <title>Blazor Maps App</title>
    <link href="_content/Syncfusion.Blazor/styles/bootstrap5.css" rel="stylesheet" />
</head>
<body>
    <!-- Your content -->
</body>
</html>
```

### Dynamic Theme Switching

Switch themes at runtime:

```csharp
@page "/maps"
@using Syncfusion.Blazor.Maps

<button @onclick="() => SwitchTheme('material')">Material</button>
<button @onclick="() => SwitchTheme('bootstrap5')">Bootstrap</button>

<div style="width: 100%; height: 600px;" id="mapContainer">
    <SfMaps @ref="mapInstance">
        <MapsLayers>
            <MapsLayer UrlTemplate="https://tile.openstreetmap.org/level/tileX/tileY.png" TValue="string">
            </MapsLayer>
        </MapsLayers>
    </SfMaps>
</div>

@code {
    private SfMaps mapInstance;

    private async Task SwitchTheme(string theme)
    {
        // Remove previous theme
        var link = new Uri($"_content/Syncfusion.Blazor/styles/{theme}.css", UriKind.Relative);
        // Update theme in your layout or use JavaScript interop
        await mapInstance.RefreshAsync();
    }
}
```

## Container Requirements

Maps need a properly sized container:

**✅ Correct - Container with explicit height:**
```html
<div style="width: 100%; height: 500px;">
    <SfMaps>...</SfMaps>
</div>
```

**❌ Incorrect - Missing height:**
```html
<SfMaps>...</SfMaps>  <!-- Collapses to 0 height -->
```

**✅ Responsive Container:**
```html
<div style="width: 100%; height: 100vh;">  <!-- Full viewport height -->
    <SfMaps>...</SfMaps>
</div>
```

## Troubleshooting Setup Issues

### Map appears blank or doesn't load

- **Check CSS import:** Verify `bootstrap5.css` (or your chosen theme) is imported
- **Check service registration:** Confirm `AddSyncfusionBlazor()` is called in Program.cs
- **Check container size:** Map must have explicit height and width
- **Check internet connection:** Tile providers (OpenStreetMap, Google Maps) require network access

### "SfMaps is not defined" or compilation error

- Confirm NuGet package is installed: `dotnet add package Syncfusion.Blazor`
- Add `@using Syncfusion.Blazor.Maps` directive
- Rebuild the project

### Map renders but tiles don't load

- Verify UrlTemplate is correct and accessible
- Check network in browser DevTools
- Ensure your ISP/firewall allows tile server access

## Key Properties for Basic Setup

| Property | Type | Example | Purpose |
|----------|------|---------|---------|
| `CenterPosition.Latitude` | double | 37.368 | Initial map center latitude |
| `CenterPosition.Longitude` | double | -122.095 | Initial map center longitude |
| `ZoomLevel` | int | 4 | Initial zoom level (1-20) |
| `UrlTemplate` | string | "https://tile.openstreetmap.org/level/tileX/tileY.png" | Tile provider URL |
| `Width` | string | "100%" | Map container width |
| `Height` | string | "600px" | Map container height |

## SfMaps Component API

### Essential Properties

```csharp
<SfMaps 
        Width="100%"
        Height="600px"
        Orientation="Orientation.Portrait"
        BaseLayerIndex="0"
        EnableLegend="true"
        Background="white"
        TabIndex="0">
</SfMaps>
```

| Property | Type | Description |
|----------|------|-------------|
| `CenterPosition` | `MapsCenterPosition` | Set initial map center |
| `ZoomLevel` | `int` | Initial zoom (1-20) |
| `Width` | `string` | Container width |
| `Height` | `string` | Container height |
| `Orientation` | `Orientation` | Portrait or Landscape |
| `BaseLayerIndex` | `int` | Index of base tile layer |
| `EnableLegend` | `bool` | Show/hide legend |
| `Background` | `string` | Background color |

### Essential Methods

```csharp

// Refresh map rendering
await mapInstance.RefreshAsync();

// Get current bounds
var bounds = await mapInstance.GetBoundsAsync();

// Export map
await mapInstance.ExportAsync(ExportType.PNG, "mymap");

// Print map
await mapInstance.PrintAsync();
```

## Next Steps

Once you have a basic map rendering, you can:
- **Add markers:** Use `<MapsMarker>` to display points of interest
- **Handle interactions:** Bind to click events and user interactions
- **Integrate map providers:** Configure Google Maps, Bing Maps, or Azure Maps
- **Add shapes and polygons:** Display geographic boundaries and regions
- **Implement data visualization:** Use color mapping for choropleth displays
- **Review complete API:** See [references/api-reference.md](api-reference.md) for all classes, methods, and properties


