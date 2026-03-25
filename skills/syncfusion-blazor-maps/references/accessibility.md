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
            <SfMaps @ref="mapInstance" AriaLabel="Zoomable and pannable map of world regions">
                <MapsLayers>
                    <MapsLayer TValue="string">
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

<SfMaps @ref="mapInstance" 
    TabIndex="0"
    AriaLabel="Interactive map, use arrow keys to pan, +/- to zoom">
    <MapsZoomSettings ZoomFactor="4"></MapsZoomSettings>
    <MapsLayers>
        <MapsLayer UrlTemplate="https://tile.openstreetmap.org/level/tileX/tileY.png" TValue="string">
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

    <SfMaps @ref="mapInstance" 
         Role="application">
         <MapsZoomSettings ZoomFactor="4"></MapsZoomSettings>
        <MapsLayers>
            <MapsLayer TValue="TemperatureData" DataSource="@TemperatureData">
                <MapsShapeSettings ColorValuePath="Temperature">
                    <MapsShapeColorMappings>
                        <MapsShapeColorMapping StartRange="-20" EndRange="0" Color='new string[] { "#4575B4" }'
                            AriaLabel="Cold regions below 0 degrees">
                        </MapsShapeColorMapping>
                        <MapsShapeColorMapping StartRange="0" EndRange="15" Color='new string[] { "#91BFDB" }'
                            AriaLabel="Cool regions 0 to 15 degrees">
                        </MapsShapeColorMapping>
                        <MapsShapeColorMapping StartRange="15" EndRange="25" Color='new string[] { "#FEE090" }'
                            AriaLabel="Warm regions 15 to 25 degrees">
                        </MapsShapeColorMapping>
                        <MapsShapeColorMapping StartRange="25" EndRange="40" Color='new string[] { "#F46D43" }'
                            AriaLabel="Hot regions 25 to 40 degrees">
                        </MapsShapeColorMapping>
                    </MapsShapeColorMappings>
                </MapsShapeSettings>
                <MapsDataLabelSettings Visible="true" LabelPath="RegionName">
                </MapsDataLabelSettings>
            </MapsLayer>
        </MapsLayers>
        <MapsLegendSettings Visible="true" 
            AriaLabel="Color legend showing temperature ranges">
        </MapsLegendSettings>
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
                    <td>@region.Density.ToString("F1") per km²</td>
                </tr>
            }
        </tbody>
    </table>
</details>

<SfMaps @ref="mapInstance" 
    >
    <MapsZoomSettings ZoomFactor="4"></MapsZoomSettings>
    <MapsLayers>
        <MapsLayer TValue="RegionData" DataSource="@RegionData">
            <MapsDataLabelSettings Visible="true" LabelPath="Name">
            </MapsDataLabelSettings>
        </MapsLayer>
    </MapsLayers>
</SfMaps>

@code {
    private SfMaps mapInstance;

    private List<RegionInfo> RegionData = new()
    {
        new RegionInfo { Name = "Region A", Population = 5000000, Density = 120 },
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
<MapsMarker Latitude="37.368" Longitude="-122.095"
    Shape="MarkerType.Circle" Width="15" Height="15"
    Tooltip="San Francisco: Population 870,000, Latitude 37.37°N, Longitude 122.10°W"
    AriaLabel="San Francisco marker, population 870,000 people">
</MapsMarker>
```

## High Contrast Mode Support

### Detect and Apply High Contrast

```csharp
@page "/high-contrast-map"
@using Syncfusion.Blazor.Maps
@inject IJSRuntime JS

<style>
    /* Default theme */
    .e-maps {
        --map-shape-color: #4575B4;
        --map-shape-border: #1a1a1a;
        --map-text-color: #000000;
        --map-bg-color: #ffffff;
    }

    /* High Contrast mode */
    @media (prefers-contrast: more) {
        .e-maps {
            --map-shape-color: #FFFF00;
            --map-shape-border: #000000;
            --map-text-color: #000000;
            --map-bg-color: #ffffff;
        }

        .e-shape {
            stroke-width: 2px;
            stroke: var(--map-shape-border);
        }

        .e-marker {
            stroke-width: 1px;
            stroke: var(--map-shape-border);
        }
    }
</style>

<SfMaps @ref="mapInstance">
<MapsZoomSettings ZoomFactor="4"></MapsZoomSettings>
    <MapsLayers>
        <MapsLayer TValue="string">
</SfMaps>

@code {
    private SfMaps mapInstance;

    protected override async Task OnAfterRenderAsync(bool firstRender)
    {
        if (firstRender)
        {
            // Check for high contrast preference
            var prefersContrast = await JS.InvokeAsync<string>(
                "window.matchMedia", "(prefers-contrast: more)");
            
            if (prefersContrast == "true")
            {
                // Apply high contrast styling
                await mapInstance.RefreshAsync();
            }
        }
    }
}
```

## Focus Management

### Focus Indicators

```csharp
<style>
    /* Visible focus indicators for all interactive elements -->
    .e-maps:focus,
    .e-marker:focus,
    .e-shape:focus {
        outline: 3px solid #4A90E2;
        outline-offset: 2px;
    }

    /* Remove default outline and replace with custom */
    button:focus {
        outline: none;
        box-shadow: 0 0 0 3px rgba(74, 144, 226, 0.5);
        border-radius: 2px;
    }
</style>

<SfMaps @ref="mapInstance" 
    TabIndex="0"
    Style="outline: none; border-radius: 4px;">
    <MapsLayers>
        <MapsLayer UrlTemplate="https://tile.openstreetmap.org/level/tileX/tileY.png" TValue="string">
        </MapsLayer>
    </MapsLayers>
</SfMaps>
```

### Focus Trap for Modal-Like Behavior

```csharp
@page "/focus-management"
@using Syncfusion.Blazor.Maps
@inject IJSRuntime JS

<button @onclick="OpenMapDetails" id="openBtn">View Map Details</button>

@if (ShowDetails)
{
    <div role="dialog" aria-modal="true" aria-labelledby="dialog-title" @ref="dialogElement">
        <h2 id="dialog-title">Map Details</h2>
        <p>Region Information</p>
        <button @onclick="() => ShowDetails = false" id="closeBtn">Close</button>
    </div>
}

<SfMaps @ref="mapInstance">
    <MapsLayers>
        <MapsLayer UrlTemplate="https://tile.openstreetmap.org/level/tileX/tileY.png" TValue="string">
        </MapsLayer>
    </MapsLayers>
</SfMaps>

@code {
    private SfMaps mapInstance;
    private bool ShowDetails = false;
    private ElementReference dialogElement;

    private async Task OpenMapDetails()
    {
        ShowDetails = true;
        await Task.Delay(100);
        // Focus first focusable element in dialog
        await JS.InvokeVoidAsync("focusElement", "closeBtn");
    }
}
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


