# Customization and Styling

> **MANDATORY SECURITY NOTICE:** Do NOT load GeoJSON, ShapeData, tile, or image resources directly from untrusted third‑party URLs at runtime. Host assets locally or return server-validated, signed URLs; validate GeoJSON/ShapeData against a strict schema, sanitize/HTML-encode properties, enforce size/complexity limits, and require human review before automated processing or forwarding to agents.

> **COMMAND_EXECUTION WARNING (IJSRuntime):** Dynamic CSS theme switching uses IJSRuntime to manipulate the DOM (adding/removing stylesheet links). Ensure theme URLs come from your application only. Never allow external or user-controlled URLs for stylesheets. Validate all CSS injection points and use CSP headers to prevent unauthorized script execution. Always use `rel="stylesheet"` and verify the href is from a trusted source before injecting via JavaScript.

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
        <!-- Use validated TileUrl variable or local tiles in production -->
        <MapsLayer UrlTemplate="@TileUrl" TValue="string">
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
<MapsFontSettings Size="12px"
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
    <MapsTitleTextStyle Size="18px"
                        FontWeight="bold"
                        Color="#333333">
    </MapsTitleTextStyle>
</MapsTitleSettings>

<MapsSubtitleSettings Text="2023 Data"
                      Alignment="Alignment.Center">
    <MapsSubtitleTextStyle Size="14px"
                           Color="#666666">
    </MapsSubtitleTextStyle>
</MapsSubtitleSettings>
```

### MapsMargin

Set space around the map.

```csharp
<SfMaps>
    <MapsMargin Left="50" Right="50" Top="50" Bottom="60" />
</SfMaps>
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

// DataLabelBorder - label outline
<MapsLayerDataLabelBorder Color="darkblue"
                  Width="2"
                  Opacity="0.8">
</MapsLayerDataLabelBorder>

// MapsShapeBorder - Shape outline
<MapsShapeBorder Color="blue"
                 Width="1"
                 Opacity="0.9">
</MapsShapeBorder>

// MapsLegendShapeBorder - legend Shape outline
<MapsLegendShapeBorder Color="blue"
                 Width="1"
                 Opacity="0.9">
</MapsLegendShapeBorder>

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

<!-- Bubble hover effect -->
<MapsBubbleHighlightSettings Enable="true"
                             Fill="red"
                             Opacity="0.8">
    <MapsBubbleHighlightBorder Color="darkred"
                               Width="2">
    </MapsBubbleHighlightBorder>
</MapsBubbleHighlightSettings>
```

### MapsLegendSettings for Styling

Configure legend appearance.

```csharp
<MapsLegendSettings Visible="true"
                    Position="LegendPosition.Bottom"
                    Arrangement="LegendArrangement.Vertical">
    <MapsLegendBorder Color="#cccccc"
                      Width="1"
                      Opacity="1">
    </MapsLegendBorder>
    <MapsLegendShapeBorder Color="#cccccc"
                      Width="1"
                      Opacity="1">
    </MapsLegendShapeBorder>
    <MapsLegendTextStyle Size="12px"
                         Color="#333333">
    </MapsLegendTextStyle>
    <MapsLegendTitle Text="Legend">
        <MapsLegendTitleStyle Size="14px"
                              FontWeight="bold">
        </MapsLegendTitleStyle>
    </MapsLegendTitle>
</MapsLegendSettings>
```

### MapsTooltipSettings

Style tooltips globally.

```csharp
<MapsTooltipSettings Visible="true">
    <MapsTooltipBorder Color="black"
                       Width="1"
                       Opacity="1">
    </MapsTooltipBorder>
    <MapsTooltipTextStyle Size="12px"
                          Color="white">
    </MapsTooltipTextStyle>
</MapsTooltipSettings>
```

### Marker Custom Styling

```csharp
@using Syncfusion.Blazor.Maps
<SfMaps>
    <MapsLayers>
        <!-- Use local or validated GeoJSON in production -->
        <MapsLayer ShapeData='new {dataOptions= "/data/world-map.json"}' TValue="string">
            <MapsMarkerSettings>
                <MapsMarker Visible="true" Datasource="@MarkerData" TValue="MapMarkerDataSource"
                    Shape="Syncfusion.Blazor.Maps.MarkerType.Circle" Width="15" Height="15">
                </MapsMarker>
            </MapsMarkerSettings>
        </MapsLayer>
    </MapsLayers>
</SfMaps>

@code {
     public class MapMarkerDataSource
    {
        public double Latitude { get; set; }
        public double Longitude { get; set; }
    };
    public List<MapMarkerDataSource> MarkerData = new List<MapMarkerDataSource>
    {
        new MapMarkerDataSource { Latitude = 47.60621, Longitude = -122.332071 }
    };
}
```

### Shape Custom Styling

```csharp
@using Syncfusion.Blazor.Maps
<SfMaps>
    <MapsLayers>
        <MapsLayer ShapeData='new {dataOptions= "/data/world-map.json"}' TValue="string">
            <MapsMarkerSettings>
                <MapsMarker Visible="true" Datasource="@MarkerData" TValue="MapMarkerDataSource"
                    Shape="Syncfusion.Blazor.Maps.MarkerType.Circle" Width="15" Height="15" Fill="lightblue">
                </MapsMarker>
            </MapsMarkerSettings>
        </MapsLayer>
    </MapsLayers>
</SfMaps>

@code {
     public class MapMarkerDataSource
    {
        public double Latitude { get; set; }
        public double Longitude { get; set; }
    };
    public List<MapMarkerDataSource> MarkerData = new List<MapMarkerDataSource>
    {
        new MapMarkerDataSource { Latitude = 47.60621, Longitude = -122.332071 }
    };
}
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
    <button @onclick='() => SwitchTheme("bootstrap5")'>Bootstrap</button>
    <button @onclick='() => SwitchTheme("material")'>Material</button>
    <button @onclick='() => SwitchTheme("fabric")'>Fabric</button>
    <button @onclick='() => SwitchTheme("tailwind")'>Tailwind</button>
</div>

<SfMaps @ref="mapInstance" Theme="Theme">
<MapsZoomSettings ZoomFactor="1"></MapsZoomSettings>
    <MapsLayers>
        <!-- Use local or validated GeoJSON in production -->
        <MapsLayer ShapeData='new {dataOptions= "/data/world-map.json"}' TValue="string">
        </MapsLayer>
    </MapsLayers>
</SfMaps>

@code {
    private SfMaps mapInstance;
    Syncfusion.Blazor.Theme Theme;

    private async Task SwitchTheme(string theme)
    {        
       Theme = theme == "bootstrap5" ? Syncfusion.Blazor.Theme.Bootstrap5 : theme == "material" ? Syncfusion.Blazor.Theme.Material : theme == "fabric" ? Syncfusion.Blazor.Theme.Fabric : Syncfusion.Blazor.Theme.Tailwind;
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
<MapsMarker DataSource="@MarkerData"
    Shape="MarkerType.Circle"
    Width="20" Height="20"
    Fill="red"
    OffsetX="20"
    OffsetY="20"
    Opacity="0.8"
    Visible="true">
</MapsMarker>
```

### Conditional Marker Styling

Style markers based on data:

```csharp
@page "/conditional-markers"
@using Syncfusion.Blazor.Maps
<SfMaps>
    <MapsLayers>
        <!-- Use local or validated GeoJSON in production -->
        <MapsLayer ShapeData='new {dataOptions= "/data/world-map.json"}' TValue="string">
            <MapsMarkerSettings>
                    <MapsMarker DataSource="@LocationOne" Visible="true"
                        Shape="Syncfusion.Blazor.Maps.MarkerType.Circle"
                        Width="20" Height="20"
                        Fill="red" TValue="LocationData">
                    </MapsMarker>
                    <MapsMarker DataSource="@LocationTwo" Visible="true"
                        Shape="Syncfusion.Blazor.Maps.MarkerType.Circle"
                        Width="20" Height="20"
                        Fill="red" TValue="LocationData">
                    </MapsMarker>
            </MapsMarkerSettings>
        </MapsLayer>
    </MapsLayers>
</SfMaps>

@code {
    private List<LocationData> LocationOne = new()
    {
        new LocationData { Latitude = 37.368, Longitude = -122.095, Name = "SF", Value = 75 },
        new LocationData { Latitude = 40.7128, Longitude = -74.0060, Name = "NYC", Value = 45 }
    };
    private List<LocationData> LocationTwo = new()
    {
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

### Shape Styling with Color

```csharp
<MapsShapeSettings>
    <MapsShapeBorder Color="darkblue" Width="2"></MapsShapeBorder>
</MapsShapeSettings>
```

## Map Controls Styling

### Custom Zoom Control Positioning

```csharp
@using Syncfusion.Blazor.Maps
<SfMaps>
<MapsZoomSettings Enable="true">
        <MapsZoomToolbarSettings BackgroundColor="pink" BorderColor="green" BorderOpacity="1" BorderWidth="3" Orientation="Syncfusion.Blazor.Maps.Orientation.Vertical" VerticalAlignment="Syncfusion.Blazor.Maps.Alignment.Near">
            <MapsZoomToolbarButton ToolbarItems="new List<Syncfusion.Blazor.Maps.ToolbarItem>() { Syncfusion.Blazor.Maps.ToolbarItem.Zoom, Syncfusion.Blazor.Maps.ToolbarItem.ZoomIn, Syncfusion.Blazor.Maps.ToolbarItem.ZoomOut, Syncfusion.Blazor.Maps.ToolbarItem.Pan, Syncfusion.Blazor.Maps.ToolbarItem.Reset }"></MapsZoomToolbarButton>
        </MapsZoomToolbarSettings>
    </MapsZoomSettings>
    <MapsLayers>
        <!-- Use local or validated GeoJSON in production -->
        <MapsLayer ShapeData='new {dataOptions= "/data/world-map.json"}' TValue="string">
        </MapsLayer>
    </MapsLayers>
</SfMaps>
```

## Dark Mode and Responsive Design

### Dark Mode Implementation

```csharp
@using Syncfusion.Blazor.Maps

<button @onclick="ToggleDarkMode">Toggle Dark Mode</button>

<SfMaps Theme="@Theme">
    <MapsLayers>
        <!-- Use local or validated GeoJSON in production -->
        <MapsLayer ShapeData='new {dataOptions= "/data/world-map.json"}' TValue="string">
        </MapsLayer>
    </MapsLayers>
</SfMaps>

@code {
    private bool IsDarkMode = false;
    private Syncfusion.Blazor.Theme Theme = Syncfusion.Blazor.Theme.Bootstrap5;
    private void ToggleDarkMode()
    {
        IsDarkMode = !IsDarkMode;
        // Toggle data-theme attribute on root element
        Theme = IsDarkMode ? Syncfusion.Blazor.Theme.Bootstrap5Dark : Syncfusion.Blazor.Theme.Bootstrap5;
        // Implementation depends on your framework setup
    }
}
```

### Responsive Container

```csharp
@using Syncfusion.Blazor.Maps
<style>
    .map-container {
        width: 100%;
        height: 100vh;
        position: relative;
    }

    /* Mobile devices (≤768px) */
    @@media (max-width: 768px) {
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
    @@media (min-width: 769px) and (max-width: 1024px) {
        .map-container {
            height: 80vh;
        }
    }

    /* Desktops (>1024px) */
    @@media (min-width: 1025px) {
        .map-container {
            height: 100vh;
        }
    }
</style>

<div class="map-container">
    <SfMaps Width="100%" Height="100%">
    <MapsZoomSettings Enable="true" ZoomFactor="4"></MapsZoomSettings>
        <MapsLayers>
            <MapsLayer ShapeData='new {dataOptions= "/data/world-map.json"}' TValue="string">
            </MapsLayer>
        </MapsLayers>
    </SfMaps>
</div>

```