## Table of Contents

- [WCAG 2.1 Compliance](#wcag-21-compliance)
   - [Semantic HTML Structure](#semantic-html-structure)
   - [Color Contrast Requirements](#color-contrast-requirements)
- [Keyboard Navigation and Shortcuts](#keyboard-navigation-and-shortcuts)
   - [Enable Keyboard Navigation](#enable-keyboard-navigation)
   - [Skip Links](#skip-links)
- [ARIA Attributes and Labels](#aria-attributes-and-labels)
   - [Comprehensive ARIA Implementation](#comprehensive-aria-implementation)
- [Screen Reader Support](#screen-reader-support)
   - [Text Alternatives for Visual Elements](#text-alternatives-for-visual-elements)
   - [Descriptive Tooltips](#descriptive-tooltips)
- [High Contrast Mode Support](#high-contrast-mode-support)
   - [Detect and Apply High Contrast](#detect-and-apply-high-contrast)
- [Focus Management](#focus-management)
   - [Focus Indicators](#focus-indicators)
   - [Focus Trap for Modal-Like Behavior](#focus-trap-for-modal-like-behavior)
- [Testing for Accessibility](#testing-for-accessibility)
   - [Accessibility Audit Checklist](#accessibility-audit-checklist)
   - [Browser DevTools Accessibility Audit](#browser-devtools-accessibility-audit)

# Accessibility and Advanced Topics

## WCAG 2.1 Compliance

### Semantic HTML Structure

Ensure map container has proper semantic markup:

```csharp
@page "/accessible-map"
@using Syncfusion.Blazor.Maps

<main>
    <section role="region" aria-label="Interactive map of population density">
        <h1>Population Density Map</h1>
        <p>This interactive map displays population density by region.</p>
        
        <div role="region" aria-live="polite" aria-label="Map container">
            <SfMaps @ref="mapInstance">
                <MapsLayers>
                    <MapsLayer ShapeData='new {dataOptions = "https://cdn.syncfusion.com/blazor/data/maps/world-map.json"}' TValue="string">
                    </MapsLayer>
                </MapsLayers>
            </SfMaps>
        </div>
    </section>
</main>

@code {
    private SfMaps mapInstance;
}
```

### Color Contrast Requirements

Ensure sufficient contrast for all map elements:

```csharp
<style>
    /* WCAG AA: Minimum 4.5:1 contrast for normal text */
    .e-maps {
        color: #000000;        /* High contrast */
        background: #FFFFFF;
    }

    /* WCAG AAA: Minimum 7:1 contrast */
    .e-tooltip {
        color: #000000;        /* Very high contrast */
        background: #FFFF00;
    }

    /* Ensure legend is readable */
    .e-legend {
        color: #1A1A1A;        /* Dark text */
        background: #FFFFFF;
    }

    /* Shape colors must have sufficient contrast */
    .e-shape {
        fill: #0066CC;         /* Accessible blue */
        stroke: #000000;       /* High contrast border */
    }

    .e-shape.high-value {
        fill: #DD0000;         /* WCAG AA compliant red */
    }
</style>
```

Use contrast checkers:
- WebAIM Contrast Checker
- WCAG Color Contrast Checker
- Chrome DevTools Accessibility audit

## Keyboard Navigation and Shortcuts

### Enable Keyboard Navigation

```csharp
@page "/keyboard-accessible"
@using Syncfusion.Blazor.Maps

<div role="region" aria-label="Map keyboard navigation controls">
    <h2>Keyboard Navigation Help</h2>
    <ul>
        <li><kbd>Tab</kbd> - Focus map controls</li>
        <li><kbd>+</kbd> - Zoom in</li>
        <li><kbd>-</kbd> - Zoom out</li>
        <li><kbd>Arrow Keys</kbd> - Pan map</li>
        <li><kbd>Home</kbd> - Reset to default view</li>
        <li><kbd>Enter</kbd> - Activate focused element</li>
    </ul>
</div>

<SfMaps>
    <MapsZoomSettings Enable="true" ZoomFactor="2"></MapsZoomSettings>
    <MapsLayers>
        <MapsLayer UrlTemplate="https://tile.openstreetmap.org/{level}/{tileX}/{tileY}.png" TValue="string">
        </MapsLayer>
    </MapsLayers>
</SfMaps>

```

### Skip Links

Allow users to skip to map content:

```html
<!-- Place at top of page -->
<a href="#map-content" class="skip-link">Skip to map content</a>

<style>
    /* Hide skip link but visible on focus */
    .skip-link {
        position: absolute;
        left: -9999px;
        z-index: 999;
    }

    .skip-link:focus {
        position: fixed;
        top: 0;
        left: 0;
        background: #000;
        color: #FFF;
        padding: 8px;
        z-index: 999;
    }
</style>
```

## ARIA Attributes and Labels

### Comprehensive ARIA Implementation

```csharp
@page "/aria-map"
@using Syncfusion.Blazor.Maps

<div role="region" aria-labelledby="map-title" aria-describedby="map-description">
    <h1 id="map-title">Global Temperature Map</h1>
    <p id="map-description">
        Interactive choropleth map showing average temperature by region. 
        Use keyboard arrows to pan, +/- to zoom. Click regions for details.
    </p>

    <SfMaps @ref="mapInstance">
         <MapsZoomSettings Enable="true" ZoomFactor="4"></MapsZoomSettings>
        <MapsLayers>
            <MapsLayer ShapeData='new {dataOptions = "https://cdn.syncfusion.com/blazor/data/maps/world-map.json"}' TValue="TemperatureRegion" DataSource="@TemperatureData">
                <MapsShapeSettings ColorValuePath="Temperature">
                    <MapsShapeColorMappings>
                        <MapsShapeColorMapping StartRange="-20" EndRange="0" Color='new string[] { "#4575B4" }' />
                        <MapsShapeColorMapping StartRange="0" EndRange="15" Color='new string[] { "#91BFDB" }' />
                        <MapsShapeColorMapping StartRange="15" EndRange="25" Color='new string[] { "#FEE090" }' />
                        <MapsShapeColorMapping StartRange="25" EndRange="40" Color='new string[] { "#F46D43" }' />
                    </MapsShapeColorMappings>
                </MapsShapeSettings>
                <MapsDataLabelSettings Visible="true" LabelPath="RegionName">
                </MapsDataLabelSettings>
            </MapsLayer>
        </MapsLayers>
        <MapsLegendSettings Visible="true" />
    </SfMaps>
</div>

@code {
    private SfMaps mapInstance;
    private List<TemperatureRegion> TemperatureData = new();

    public class TemperatureRegion
    {
        public string RegionName { get; set; }
        public double Temperature { get; set; }
    }
}
```

## Screen Reader Support

### Text Alternatives for Visual Elements

```csharp
@page "/screen-reader-friendly"
@using Syncfusion.Blazor.Maps

<!-- Alternative text representation -->
<details>
    <summary>Text description of map (for screen readers)</summary>
    <table>
        <thead>
            <tr>
                <th>Region</th>
                <th>Population</th>
                <th>Density</th>
            </tr>
        </thead>
        <tbody>
            @foreach (var region in RegionData)
            {
                <tr>
                    <td>@region.Name</td>
                    <td>@region.Population.ToString("N0")</td>
                    <td>@region.Density.ToString("F1") per km</td>
                </tr>
            }
        </tbody>
    </table>
</details>

<SfMaps @ref="mapInstance">
    <MapsZoomSettings ZoomFactor="1"></MapsZoomSettings>
    <MapsLayers>
        <MapsLayer ShapeData='new {dataOptions= "https://cdn.syncfusion.com/maps/map-data/usa.json"}' TValue="RegionInfo" DataSource="@RegionData">
            <MapsDataLabelSettings Visible="true" LabelPath="name">
            </MapsDataLabelSettings>
        </MapsLayer>
    </MapsLayers>
</SfMaps>

@code {
    private SfMaps mapInstance;

    private List<RegionInfo> RegionData = new()
    {
        new RegionInfo { Name = "Texas", Population = 5000000, Density = 120 },
        new RegionInfo { Name = "Region B", Population = 3000000, Density = 85 }
    };

    public class RegionInfo
    {
        public string Name { get; set; }
        public int Population { get; set; }
        public double Density { get; set; }
    }
}
```

### Descriptive Tooltips

```csharp
<MapsMarker Visible="true" Shape="Syncfusion.Blazor.Maps.MarkerType.Circle" Fill="white" Width="20"
                DataSource="HighestPopulation" TValue="City">
        <MapsMarkerBorder Width="2" Color="#333"></MapsMarkerBorder>
        <MapsMarkerTooltipSettings Visible="true" ValuePath="Name"></MapsMarkerTooltipSettings>
    </MapsMarker>
</MapsMarkerSettings>
```

## Testing for Accessibility

### Accessibility Audit Checklist

- [ ] **Keyboard Navigation:** All interactive elements accessible via Tab/Shift+Tab
- [ ] **Focus Indicators:** Clear visual focus indicator on all elements
- [ ] **Color Contrast:** All text meets WCAG AA (4.5:1) minimum
- [ ] **ARIA Labels:** All regions and interactive elements properly labeled
- [ ] **Alt Text:** Meaningful descriptions for all visual content
- [ ] **Heading Structure:** Proper heading hierarchy (h1 > h2 > h3)
- [ ] **Link Text:** Links have descriptive text (avoid "click here")
- [ ] **Form Labels:** All inputs associated with labels
- [ ] **Error Messages:** Clear, helpful error descriptions
- [ ] **Skip Links:** Users can skip repetitive content
- [ ] **Zoom:** Page works at 200% zoom
- [ ] **Motion:** No content that flashes more than 3 times/second
- [ ] **Screen Reader:** Tested with NVDA, JAWS, or VoiceOver

### Browser DevTools Accessibility Audit

```javascript
// Run in browser console
// Chrome: Lighthouse, Edge: DevTools Accessibility
// Firefox: WAVE extension
// All browsers: axe DevTools extension
```


