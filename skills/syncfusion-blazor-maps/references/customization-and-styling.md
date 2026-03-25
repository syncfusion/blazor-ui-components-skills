# Customization and Styling

## Table of Contents
- [Themes and Visual Styles](#themes-and-visual-styles)
   - [SfMaps Theme Property (NEW - Previously Missing)](#sfmaps-theme-property-new---previously-missing)
- [CSS Class Customization](#css-class-customization)
   - [Targeting Map Elements with CSS](#targeting-map-elements-with-css)
- [API Reference for Styling](#api-reference-for-styling)
   - [MapsFontSettings](#mapsfontsettings)
   - [MapsTitleSettings & MapsSubtitleSettings](#mapstitlesettings-mapssubtitlesettings)
   - [MapsMargin](#mapsmargin)
   - [Border Classes](#border-classes)
   - [MapsAreaSettings](#mapsareasettings)
   - [Highlight and Selection Settings](#highlight-and-selection-settings)
   - [MapsLegendSettings for Styling](#mapslegendsettings-for-styling)
   - [MapsTooltipSettings](#mapstooltipsettings)
- [CSS Class Customization](#css-class-customization)
   - [Marker Custom Styling](#marker-custom-styling)
   - [Shape Custom Styling](#shape-custom-styling)
- [Theme Studio Integration](#theme-studio-integration)
   - [Using Default Themes](#using-default-themes)
   - [Switching Themes Dynamically](#switching-themes-dynamically)
   - [Theme Studio Custom Themes](#theme-studio-custom-themes)
- [Marker and Shape Styling](#marker-and-shape-styling)
   - [Individual Marker Styling](#individual-marker-styling)
   - [Conditional Marker Styling](#conditional-marker-styling)
   - [Shape Styling with Color Gradients](#shape-styling-with-color-gradients)
- [Map Controls Styling](#map-controls-styling)
   - [Navigation Buttons](#navigation-buttons)
   - [Custom Control Positioning](#custom-control-positioning)
- [Dark Mode and Responsive Design](#dark-mode-and-responsive-design)
   - [Dark Mode Implementation](#dark-mode-implementation)
   - [Responsive Container](#responsive-container)


## Themes and Visual Styles

### SfMaps Theme Property (NEW - Previously Missing)

Apply predefined visual themes to your maps:

```csharp
<SfMaps Theme="Theme.Bootstrap5">
    <MapsLayers>
        <MapsLayer UrlTemplate="https://tile.openstreetmap.org/level/tileX/tileY.png" TValue="string">
        </MapsLayer>
    </MapsLayers>
</SfMaps>
```

**Available Themes:**

| Theme | Description | Best For |
|-------|-------------|----------|
| `Theme.Material` | Material Design theme | Modern apps, Google-inspired look |
| `Theme.Fabric` | Microsoft Fabric design | Microsoft ecosystem integration |
| `Theme.Bootstrap` | Bootstrap 4 theme | Bootstrap-based applications |
| `Theme.Bootstrap4` | Bootstrap 4 specific | Legacy Bootstrap 4 projects |
| `Theme.Bootstrap5` | Bootstrap 5 theme | Latest Bootstrap projects |
| `Theme.HighContrast` | High contrast theme | Accessibility, low-vision users |
| `Theme.FluentDark` | Fluent Dark theme | Dark mode applications, Microsoft Fluent |

**Theme Application:**
```csharp
// In your Blazor component
<SfMaps Theme="Theme.Bootstrap5" Width="100%" Height="600px">
    <!-- Map configuration -->
</SfMaps>
```

**Themeing with Multiple Components:**
```csharp
<!-- Apply theme to all Syncfusion components in layout -->
<!-- In App.razor or Layout.razor -->
<SyncfusionBlazor Theme="Bootstrap5">
    <!-- All Syncfusion components use Bootstrap5 theme -->
    @Body
</SyncfusionBlazor>
```

---

## CSS Class Customization

### Targeting Map Elements with CSS

The Syncfusion Maps component renders with specific CSS classes you can override:

```html
<!-- In your CSS file or style block -->
<style>
    /* Target the main map container */
    .e-maps {
        background-color: #f5f5f5;
        border-radius: 8px;
        box-shadow: 0 2px 8px rgba(0,0,0,0.1);
    }

    /* Target map layers */
    .e-layer {
        opacity: 0.9;
    }

    /* Target markers */
    .e-marker {
        filter: drop-shadow(0 2px 4px rgba(0,0,0,0.3));
    }

    /* Target shapes (polygons) */
    .e-shape {
        transition: fill 0.3s ease;
    }

    .e-shape:hover {
        opacity: 0.8;
    }

    /* Target tooltips -->
    .e-tooltip {
        background-color: rgba(0,0,0,0.8) !important;
        color: white;
        padding: 8px 12px;
        border-radius: 4px;
    }
```

## API Reference for Styling

### MapsFontSettings

Configure text appearance globally or for specific elements.

```csharp
<MapsFontSettings FontSize="12px"
                  FontFamily="Arial, sans-serif"
                  FontStyle="normal"
                  FontWeight="normal"
                  Color="#333333"
                  Opacity="1">
</MapsFontSettings>
```

| Property | Type | Description |
|----------|------|-------------|
| `FontSize` | string | Text size (e.g., "12px", "1em") |
| `FontFamily` | string | Font name or stack |
| `FontStyle` | string | normal, italic, oblique |
| `FontWeight` | string | normal, bold, lighter, or 100-900 |
| `Color` | string | Text color (hex or named) |
| `Opacity` | double | Text transparency (0-1) |

### MapsTitleSettings & MapsSubtitleSettings

Configure title and subtitle appearance.

```csharp
<MapsTitleSettings Text="World Population Map"
                   Alignment="Alignment.Center"
                   Description="Population density by country">
    <MapsTitleTextStyle FontSize="18px"
                        FontWeight="bold"
                        Color="#333333">
    </MapsTitleTextStyle>
</MapsTitleSettings>

<MapsSubtitleSettings Text="2023 Data"
                      Alignment="Alignment.Center">
    <MapsSubtitleTextStyle FontSize="14px"
                           Color="#666666">
    </MapsSubtitleTextStyle>
</MapsSubtitleSettings>
```

### MapsMargin

Set space around the map.

```csharp
<SfMaps Margin="@mapMargin">
</SfMaps>

@code {
    private MapsMargin mapMargin = new MapsMargin
    {
        Top = 10,
        Bottom = 10,
        Left = 10,
        Right = 10
    };
}
```

### Border Classes

Configure borders for various elements.

```csharp
// MapsBorder - General border styling
<MapsBorder Color="#cccccc"
            Width="1"
            Opacity="1">
</MapsBorder>

// MapsMarkerBorder - Marker outline
<MapsMarkerBorder Color="darkblue"
                  Width="2"
                  Opacity="0.8">
</MapsMarkerBorder>

// MapsShapeBorder - Shape outline
<MapsShapeBorder Color="blue"
                 Width="1"
                 Opacity="0.9">
</MapsShapeBorder>

// MapsBubbleBorder - Bubble outline
<MapsBubbleBorder Color="blue"
                  Width="1"
                  Opacity="0.8">
</MapsBubbleBorder>
```

### MapsAreaSettings

Configure map background and outer border.

```csharp
<MapsAreaSettings Background="white">
    <MapsAreaBorder Color="#cccccc"
                    Width="2"
                    Opacity="1">
    </MapsAreaBorder>
</MapsAreaSettings>
```

### Highlight and Selection Settings

Configure interactive element styling.

```csharp
<!-- Highlight on hover -->
<MapsLayerHighlightSettings Enable="true"
                            Fill="yellow"
                            Opacity="0.5">
    <MapsLayerHighlightBorder Color="orange"
                              Width="2">
    </MapsLayerHighlightBorder>
</MapsLayerHighlightSettings>

<!-- Selection styling -->
<MapsLayerSelectionSettings Enable="true"
                            Fill="cyan"
                            Opacity="0.7">
    <MapsLayerSelectionBorder Color="blue"
                              Width="3">
    </MapsLayerSelectionBorder>
</MapsLayerSelectionSettings>

<!-- Marker hover effect -->
<MapsMarkerHighlightSettings Enable="true"
                             Fill="red"
                             Opacity="0.8">
    <MapsMarkerHighlightBorder Color="darkred"
                               Width="2">
    </MapsMarkerHighlightBorder>
</MapsMarkerHighlightSettings>
```

### MapsLegendSettings for Styling

Configure legend appearance.

```csharp
<MapsLegendSettings Visible="true"
                    Position="LegendPosition.BottomRight"
                    Arrangement="LegendArrangement.Vertical">
    <MapsLegendBorder Color="#cccccc"
                      Width="1"
                      Opacity="1">
    </MapsLegendBorder>
    <MapsLegendTextStyle FontSize="12px"
                         Color="#333333">
    </MapsLegendTextStyle>
    <MapsLegendTitle Text="Legend">
        <MapsLegendTitleStyle FontSize="14px"
                              FontWeight="bold">
        </MapsLegendTitleStyle>
    </MapsLegendTitle>
</MapsLegendSettings>
```

### MapsTooltipSettings

Style tooltips globally.

```csharp
<MapsTooltipSettings Visible="true"
                     Gesture="TooltipGesture.Move"
                     EnableAnimation="true">
    <MapsTooltipBorder Color="black"
                       Width="1"
                       Opacity="1">
    </MapsTooltipBorder>
    <MapsTooltipTextStyle FontSize="12px"
                          Color="white">
    </MapsTooltipTextStyle>
</MapsTooltipSettings>
```

## CSS Class Customization

    /* Target legend -->
    .e-legend {
        background: white;
        border: 1px solid #ddd;
        border-radius: 4px;
        padding: 10px;
    }
</style>
```

### Marker Custom Styling

```csharp
<style>
    .custom-marker {
        transition: transform 0.2s;
    }

    .custom-marker:hover {
        transform: scale(1.2);
    }

    .custom-marker.selected {
        filter: drop-shadow(0 0 8px gold);
    }
</style>

<SfMaps>
    <MapsLayers>
        <MapsLayer UrlTemplate="https://tile.openstreetmap.org/level/tileX/tileY.png" TValue="string">
            <MapsMarkerSettings>
                <MapsMarker Latitude="37.368" Longitude="-122.095"
                    CssClass="custom-marker"
                    Shape="MarkerType.Circle" Width="15" Height="15">
                </MapsMarker>
            </MapsMarkerSettings>
        </MapsLayer>
    </MapsLayers>
</SfMaps>
```

### Shape Custom Styling

```csharp
<style>
    .region-highlight {
        transition: all 0.3s ease;
        opacity: 0.7;
    }

    .region-highlight:hover {
        opacity: 1;
        stroke-width: 2;
    }

    .region-highlight.high-value {
        fill: #d32f2f;
    }

    .region-highlight.medium-value {
        fill: #f57c00;
    }

    .region-highlight.low-value {
        fill: #388e3c;
    }
</style>

<SfMaps>
    <MapsLayers>
        <MapsLayer TValue="StateData" DataSource="@StateData">
            <MapsShapeSettings Fill="lightblue"
                CssClass="region-highlight">
            </MapsShapeSettings>
        </MapsLayer>
    </MapsLayers>
</SfMaps>
```

## Theme Studio Integration

### Using Default Themes

Syncfusion provides pre-built themes. Import in your layout:

```html
<!-- Bootstrap 5 theme (default) -->
<link href="_content/Syncfusion.Blazor/styles/bootstrap5.css" rel="stylesheet" />

<!-- Alternative themes -->
<!-- <link href="_content/Syncfusion.Blazor/styles/material.css" rel="stylesheet" /> -->
<!-- <link href="_content/Syncfusion.Blazor/styles/fabric.css" rel="stylesheet" /> -->
<!-- <link href="_content/Syncfusion.Blazor/styles/tailwind.css" rel="stylesheet" /> -->
<!-- <link href="_content/Syncfusion.Blazor/styles/bootstrap4.css" rel="stylesheet" /> -->
```

### Switching Themes Dynamically

```csharp
@page "/theme-switcher"
@using Syncfusion.Blazor.Maps
@inject IJSRuntime JS

<div style="margin-bottom: 20px;">
    <button @onclick="() => SwitchTheme('bootstrap5')">Bootstrap</button>
    <button @onclick="() => SwitchTheme('material')">Material</button>
    <button @onclick="() => SwitchTheme('fabric')">Fabric</button>
    <button @onclick="() => SwitchTheme('tailwind')">Tailwind</button>
</div>

<SfMaps @ref="mapInstance" >
<MapsZoomSettings ZoomFactor="4"></MapsZoomSettings>
    <MapsLayers>
        <MapsLayer UrlTemplate="https://tile.openstreetmap.org/level/tileX/tileY.png" TValue="string">
        </MapsLayer>
    </MapsLayers>
</SfMaps>

@code {
    private SfMaps mapInstance;

    private async Task SwitchTheme(string theme)
    {
        // Remove existing theme link
        await JS.InvokeVoidAsync("removeThemeLink");

        // Add new theme
        string cssUrl = $"_content/Syncfusion.Blazor/styles/{theme}.css";
        await JS.InvokeVoidAsync("addThemeLink", cssUrl);

        // Refresh map
        await mapInstance.RefreshAsync();
    }
}
```

**JavaScript helper (in your wwwroot/js/app.js):**
```javascript
window.removeThemeLink = function() {
    const link = document.querySelector('link[rel="stylesheet"][href*="Syncfusion"]');
    if (link) {
        link.remove();
    }
};

window.addThemeLink = function(url) {
    const link = document.createElement('link');
    link.rel = 'stylesheet';
    link.href = url;
    document.head.appendChild(link);
};
```

### Theme Studio Custom Themes

Create custom themes with Syncfusion Theme Studio:

1. Go to [Theme Studio](https://www.syncfusion.com/themestudio/blazor)
2. Customize colors, fonts, spacing
3. Export CSS file
4. Add to your wwwroot/css/ folder

```html
<!-- Your custom theme -->
<link href="css/custom-theme.css" rel="stylesheet" />
```

## Marker and Shape Styling

### Individual Marker Styling

```csharp
<MapsMarker Latitude="37.368" Longitude="-122.095"
    Shape="MarkerType.Circle"
    Width="20" Height="20"
    Fill="red"
    BorderColor="darkred"
    BorderWidth="2"
    Opacity="0.8"
    Tooltip="San Francisco">
</MapsMarker>
```

### Conditional Marker Styling

Style markers based on data:

```csharp
@page "/conditional-markers"
@using Syncfusion.Blazor.Maps

<SfMaps>
<MapsZoomSettings ZoomFactor="4"></MapsZoomSettings>
    <MapsLayers>
        <MapsLayer UrlTemplate="https://tile.openstreetmap.org/level/tileX/tileY.png" TValue="string">
            <MapsMarkerSettings>
                @foreach (var location in Locations)
                {
                    var markerColor = location.Value > 50 ? "red" : 
                                     location.Value > 25 ? "orange" : "green";
                    var markerSize = location.Value > 50 ? 20 : 15;

                    <MapsMarker Latitude="@location.Latitude" 
                        Longitude="@location.Longitude"
                        Shape="MarkerType.Circle"
                        Width="@markerSize" Height="@markerSize"
                        Fill="@markerColor"
                        Tooltip="@location.Name">
                    </MapsMarker>
                }
            </MapsMarkerSettings>
        </MapsLayer>
    </MapsLayers>
</SfMaps>

@code {
    private int ZoomLevel = 4;

    private List<LocationData> Locations = new()
    {
        new LocationData { Latitude = 37.368, Longitude = -122.095, Name = "SF", Value = 75 },
        new LocationData { Latitude = 40.7128, Longitude = -74.0060, Name = "NYC", Value = 45 },
        new LocationData { Latitude = 34.0522, Longitude = -118.2437, Name = "LA", Value = 20 }
    };

    public class LocationData
    {
        public double Latitude { get; set; }
        public double Longitude { get; set; }
        public string Name { get; set; }
        public int Value { get; set; }
    }
}
```

### Shape Styling with Color Gradients

```csharp
<MapsShapeSettings>
                    <MapsShapeBorder Color="darkblue" Width="2"></MapsShapeBorder>
                </MapsShapeSettings>

<!-- Define SVG gradient -->
<svg width="0" height="0">
    <defs>
        <linearGradient id="shapeGradient" x1="0%" y1="0%" x2="100%" y2="100%">
            <stop offset="0%" style="stop-color:lightblue;stop-opacity:1" />
            <stop offset="100%" style="stop-color:darkblue;stop-opacity:1" />
        </linearGradient>
    </defs>
</svg>
```

## Map Controls Styling

### Navigation Buttons

```csharp
<style>
    .e-nav-button {
        background-color: #007bff;
        color: white;
        border-radius: 4px;
        padding: 8px 12px;
        margin: 4px;
    }

    .e-nav-button:hover {
        background-color: #0056b3;
    }

    .e-nav-button:disabled {
        opacity: 0.5;
        cursor: not-allowed;
    }
</style>

<SfMaps NavigationButtons="true">
    <MapsLayers>
        <MapsLayer UrlTemplate="https://tile.openstreetmap.org/level/tileX/tileY.png" TValue="string">
        </MapsLayer>
    </MapsLayers>
</SfMaps>
```

### Custom Control Positioning

```csharp
<style>
    .map-controls-custom {
        position: absolute;
        top: 20px;
        right: 20px;
        display: flex;
        flex-direction: column;
        gap: 10px;
        z-index: 100;
    }

    .control-button {
        background: white;
        border: 1px solid #ddd;
        border-radius: 4px;
        padding: 10px 15px;
        cursor: pointer;
        box-shadow: 0 2px 4px rgba(0,0,0,0.1);
    }

    .control-button:hover {
        background: #f5f5f5;
    }
</style>

<div class="map-controls-custom">
    <button class="control-button" @onclick="ZoomIn">+</button>
    <button class="control-button" @onclick="ZoomOut">−</button>
    <button class="control-button" @onclick="Reset">⟲</button>
</div>

<SfMaps @ref="mapInstance" >
<MapsZoomSettings ZoomFactor="4"></MapsZoomSettings>
    <MapsLayers>
        <MapsLayer UrlTemplate="https://tile.openstreetmap.org/level/tileX/tileY.png" TValue="string">
        </MapsLayer>
    </MapsLayers>
</SfMaps>

```

## Dark Mode and Responsive Design

### Dark Mode Implementation

```csharp
<style>
    :root {
        --map-bg: white;
        --map-text: black;
        --map-border: #ddd;
    }

    :root[data-theme="dark"] {
        --map-bg: #1e1e1e;
        --map-text: white;
        --map-border: #444;
    }

    .e-maps {
        background-color: var(--map-bg);
        color: var(--map-text);
    }

    .e-legend {
        background-color: var(--map-bg);
        border-color: var(--map-border);
        color: var(--map-text);
    }

    .e-tooltip {
        background-color: var(--map-bg) !important;
        color: var(--map-text);
        border: 1px solid var(--map-border);
    }
</style>

@page "/dark-mode"
@using Syncfusion.Blazor.Maps

<button @onclick="ToggleDarkMode">Toggle Dark Mode</button>

<SfMaps >
<MapsZoomSettings ZoomFactor="4"></MapsZoomSettings>
    <MapsLayers>
        <MapsLayer UrlTemplate="https://tile.openstreetmap.org/level/tileX/tileY.png" TValue="string">
        </MapsLayer>
    </MapsLayers>
</SfMaps>

@code {
    private bool IsDarkMode = false;

    private async Task ToggleDarkMode()
    {
        IsDarkMode = !IsDarkMode;
        // Toggle data-theme attribute on root element
        var theme = IsDarkMode ? "dark" : "light";
        // Implementation depends on your framework setup
    }
}
```

### Responsive Container

```csharp
<style>
    .map-container {
        width: 100%;
        height: 100vh;
        position: relative;
    }

    /* Mobile devices (≤768px) */
    @media (max-width: 768px) {
        .map-container {
            height: 70vh;
        }

        .map-controls-custom {
            top: 10px;
            right: 10px;
        }

        .control-button {
            padding: 8px 12px;
            font-size: 14px;
        }
    }

    /* Tablets (769px - 1024px) */
    @media (min-width: 769px) and (max-width: 1024px) {
        .map-container {
            height: 80vh;
        }
    }

    /* Desktops (>1024px) */
    @media (min-width: 1025px) {
        .map-container {
            height: 100vh;
        }
    }
</style>

<div class="map-container">
    <SfMaps >
    <MapsZoomSettings ZoomFactor="4"></MapsZoomSettings>
        <MapsLayers>
            <MapsLayer UrlTemplate="https://tile.openstreetmap.org/level/tileX/tileY.png" TValue="string">
            </MapsLayer>
        </MapsLayers>
    </SfMaps>
</div>

```


