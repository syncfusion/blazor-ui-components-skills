---
name: syncfusion-blazor-maps
description: Implement interactive spatial data visualization with Syncfusion Maps component for Blazor. Use this skill when user needs to display geographic data, add markers/polygons to maps, integrate map providers (Google Maps, Bing Maps, Azure Maps, OpenStreetMap), create choropleth visualizations, handle user interactions with maps, export/print maps, support internationalization, implement accessibility features, or customize map styling and appearance.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
  category: "Data Visualization"
---

# Implementing Syncfusion Maps for Blazor

A comprehensive guide to implementing Syncfusion Maps component in Blazor applications. Syncfusion Maps provides powerful spatial visualization capabilities including marker management, polygon overlays, layer support, event handling, and integration with multiple map providers.

## When to Use This Skill

Use Syncfusion Maps when you need to:
- Display geographic data on interactive maps
- Visualize data across regions using color mapping (choropleth)
- Add markers, polygons, and annotations to maps
- Handle user interactions (click, hover, zoom, pan)
- Support multiple map providers (Google, Bing, Azure, OpenStreetMap)
- Export or print maps with custom legends and labels
- Build location-aware applications
- Create multi-layer spatial visualizations

## Component Overview

Syncfusion Maps is a powerful geospatial visualization component that enables you to:
- Render interactive maps with multiple provider options
- Bind and visualize geographic datasets
- Support rich spatial features (markers, polygons, layers, annotations)
- Handle complex user interactions and events
- Customize appearance through themes and styling
- Enable localization and accessibility features
- Export maps for reporting and sharing

## Documentation and Navigation Guide

Choose the reference guide that matches your current task:

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Installation and NuGet package setup
- Basic map initialization and rendering
- CSS imports and theme configuration
- Creating your first interactive map
- Step-by-step setup walkthrough

### Map Providers and Configuration
📄 **Read:** [references/map-providers.md](references/map-providers.md)
- Google Maps setup and API key configuration
- Bing Maps setup and authentication
- Azure Maps provider configuration
- OpenStreetMap setup
- Provider comparison and selection guide

### Markers and Layers
📄 **Read:** [references/markers-and-layers.md](references/markers-and-layers.md)
- Adding and managing markers
- Marker clustering and grouping
- Working with layers and layer collections
- Toggling layer visibility
- Dynamic marker updates and data binding

### Spatial Features and Overlays
📄 **Read:** [references/spatial-features.md](references/spatial-features.md)
- Drawing polygons and geographic shapes
- Creating navigation lines and polylines
- Adding annotations with text, icons, and circles
- Creating data bubbles and interactive overlays
- Advanced spatial geometry features

### Data Visualization and Mapping
📄 **Read:** [references/data-visualization.md](references/data-visualization.md)
- Color mapping and choropleth visualization
- Configuring legends and legend placement
- Data labels on map elements
- Populating maps with geographic datasets
- Advanced data binding and visualization patterns

### User Interactions
📄 **Read:** [references/user-interactions.md](references/user-interactions.md)
- Handling mouse clicks and double-clicks
- Zoom and pan control configuration
- Tooltip and popup behavior
- Keyboard navigation support
- Custom interaction patterns

### Events and Methods
📄 **Read:** [references/events-and-methods.md](references/events-and-methods.md)
- Mouse event handling (click, hover, move)
- Pan and zoom event capture
- Programmatic map methods (pan, zoom, reset, refresh)
- Event data and callback patterns
- Triggering actions from user interactions

### Customization and Styling
📄 **Read:** [references/customization-and-styling.md](references/customization-and-styling.md)
- CSS class customization
- Theme Studio integration
- Marker and popup styling
- Map controls and navigation styling
- Dark mode and responsive design

### Print and Export
📄 **Read:** [references/print-and-export.md](references/print-and-export.md)
- Exporting maps as PNG, SVG, and PDF
- Print functionality and page setup
- Exporting with legends and data labels
- File format considerations
- Server-side and client-side export options

### Internationalization and Localization
📄 **Read:** [references/internationalization-and-localization.md](references/internationalization-and-localization.md)
- Multi-language support for map labels
- Right-to-left (RTL) text support
- Localized number and date formatting
- Regional map variations
- Language-specific customization

### State Persistence
📄 **Read:** [references/state-persistence.md](references/state-persistence.md)
- Saving and restoring map state
- Persisting zoom level and center position
- Preserving user interaction state
- Session and local storage integration
- State management patterns

### Accessibility and Advanced Topics
📄 **Read:** [references/accessibility.md](references/accessibility.md)
- WCAG 2.1 compliance and standards
- Keyboard navigation and shortcuts
- ARIA attributes and semantic markup
- Screen reader support
- High contrast mode support
- Assistive technology compatibility

### Complete API Reference
📄 **Read:** [references/api-reference.md](references/api-reference.md)
- `SfMaps` main component properties and methods
- Configuration classes (`MapsCenterPosition`, `MapsZoomSettings`, `MapsLegendSettings`, etc.)
- Event arguments for all event types
- Interfaces (`ILayer`, `IMarker`, `IBubble`)
- Enumerations for `MarkerType`, `ExportType`, `ProjectionType`, `GeometryType`, etc.
- Properties quick reference guide
- Complete class hierarchy and API surface

## Quick Start Example

```csharp
// Basic map setup in Blazor
@page "/maps-demo"
@using Syncfusion.Blazor.Maps

<SfMaps>
    <MapsLayers>
        <MapsLayer UrlTemplate="https://tile.openstreetmap.org/level/tileX/tileY.png">
        </MapsLayer>
    </MapsLayers>
</SfMaps>

@code {
    // Additional configuration as needed
}
```

## Common Patterns

### Pattern 1: Adding Markers to a Map
```csharp
<SfMaps>
    <MapsLayers>
        <MapsLayer TValue="MarkerData" UrlTemplate="https://tile.openstreetmap.org/level/tileX/tileY.png">
            <MapsMarkerSettings>
                <MapsMarker TValue="MarkerData" Latitude="37.368" Longitude="-122.095" 
                    Width="15" Height="15">
                </MapsMarker>
            </MapsMarkerSettings>
        </MapsLayer>
    </MapsLayers>
</SfMaps>

@code {
    public class MarkerData
    {
        public double Latitude { get; set; }
        public double Longitude { get; set; }
    }
}
```

### Pattern 2: Binding Data with Color Mapping
```csharp
<SfMaps>
    <MapsLayers>
        <MapsLayer ShapeDataSource="@ShapeData" 
                   ShapePropertyPath="@ShapePropertyPath"
                   DataSource="@DataSource" TValue="DataType">
            <MapsShapeSettings ColorValuePath="Population">
                <MapsShapeColorMappings>
                    <MapsShapeColorMapping From="0" To="50000" Color="#B3E5FC"></MapsShapeColorMapping>
                    <MapsShapeColorMapping From="50000" To="100000" Color="#81D4FA"></MapsShapeColorMapping>
                </MapsShapeColorMappings>
            </MapsShapeSettings>
        </MapsLayer>
    </MapsLayers>
</SfMaps>
```

### Pattern 3: Handling Map Click Events
```csharp
<SfMaps @ref="mapInstance" OnShapeSelected="ShapeSelected" 
        OnMarkerClick="MarkerClicked">
    <MapsLayers>
        <MapsLayer UrlTemplate="https://tile.openstreetmap.org/level/tileX/tileY.png">
        </MapsLayer>
    </MapsLayers>
</SfMaps>

@code {
    private SfMaps mapInstance;

    private void MarkerClicked(MarkerClickEventArgs args)
    {
        Console.WriteLine($"Marker clicked at: {args.Latitude}, {args.Longitude}");
    }

    private void ShapeSelected(ShapeSelectedEventArgs args)
    {
        Console.WriteLine($"Shape selected: {args.Data}");
    }
}
```

## Key Features Summary

- **Multiple Map Providers:** Support for Google Maps, Bing Maps, Azure Maps, and OpenStreetMap
- **Rich Geospatial Features:** Markers, polygons, polylines, annotations, and bubbles
- **Data Visualization:** Color mapping, choropleth support, legends, and data labels
- **Interactivity:** Click handling, hover tooltips, zoom/pan controls, keyboard navigation
- **Layer Management:** Multi-layer support with visibility toggling and dynamic updates
- **Export Capabilities:** PNG, SVG, and PDF export with legends and labels
- **Internationalization:** Multi-language support and RTL text rendering
- **Accessibility:** WCAG compliance with keyboard navigation and screen reader support
- **Customization:** Theme Studio integration, CSS customization, marker styling
- **Performance:** Optimized rendering for large datasets and marker clustering

