## Table of Contents

- [Handling Mouse Clicks](#handling-mouse-clicks)
   - [Click on Markers](#click-on-markers)
   - [Click on Shapes (Polygons)](#click-on-shapes-polygons)
   - [Double-Click Events](#double-click-events)
- [Zoom and Pan Controls](#zoom-and-pan-controls)
   - [Enable/Disable Zoom](#enabledisable-zoom)
   - [Programmatic Zoom](#programmatic-zoom)
- [Tooltip and Popup Handling](#tooltip-and-popup-handling)
   - [Basic Tooltip](#basic-tooltip)
   - [Tooltip on Hover](#tooltip-on-hover)
   - [Custom Popup on Click](#custom-popup-on-click)
- [Keyboard Navigation](#keyboard-navigation)
   - [Keyboard Controls](#keyboard-controls)
   - [Custom Keyboard Handlers](#custom-keyboard-handlers)
- [Custom Interaction Patterns](#custom-interaction-patterns)
   - [Selection State Management](#selection-state-management)
   - [Drag to Pan](#drag-to-pan)
- [Sanitizing External Content](#sanitizing-external-content)
   - [Understanding the Risk](#understanding-the-risk)
   - [Safe Data Binding Patterns](#safe-data-binding-patterns)
   - [Preventing Prompt Injection](#preventing-prompt-injection)
   - [Content Security Policy for Maps](#content-security-policy-for-maps)
   - [Security Checklist](#security-checklist)

# User Interactions

## Handling Mouse Clicks

### Click on Markers

```csharp
@page "/marker-click"
@using Syncfusion.Blazor.Maps

<div>Last clicked: @ClickedMarkerInfo</div>

<SfMaps @ref="mapInstance">
    <MapsEvents MarkerClick="MarkerClickHandler" />
    <MapsLayers>
        <MapsLayer ShapeData='new {dataOptions= "https://cdn.syncfusion.com/maps/map-data/world-map.json"}' TValue="string">
            <MapsMarkerSettings>
                <MapsMarker Visible="true" DataSource="California" Height="25" Width="15" TValue="City"></MapsMarker>
                <MapsMarker Visible="true" DataSource="NewYork" Height="25" Width="15" TValue="City"></MapsMarker>
                <MapsMarker Visible="true" DataSource="Iowa" Height="25" Width="15" TValue="City"></MapsMarker>
            </MapsMarkerSettings>
        </MapsLayer>
    </MapsLayers>
</SfMaps>

@code {
    private SfMaps mapInstance;
    private string ClickedMarkerInfo = "None";
    public class City
    {
        public double Latitude { get; set; }
        public double Longitude { get; set; }
    }
    public List<City> California = new List<City> {
        new City {Latitude = 35.145083,Longitude = -117.960260}
    };
    public List<City> NewYork = new List<City> {
        new City { Latitude = 40.724546, Longitude = -73.850344 }
    };
    public List<City> Iowa = new List<City> {
        new City {Latitude = 41.657782, Longitude = -91.533857}
    };
    private void MarkerClickHandler(MarkerClickEventArgs args)
    {
        ClickedMarkerInfo = $"Latitude: {args.Latitude}, Longitude: {args.Longitude}";
    }
}
```

### Click on Shapes (Polygons)

```csharp
@using Syncfusion.Blazor.Maps
<SfMaps>
    <MapsEvents ShapeSelected="@ShapeSelectedEvent"></MapsEvents>
    <MapsLayers>
        <MapsLayer ShapeData='new {dataOptions= "https://cdn.syncfusion.com/maps/map-data/world-map.json"}' TValue="string">
            <MapsLayerSelectionSettings Enable="true" Fill="green">
                <MapsLayerSelectionBorder Color="White" Width="2"></MapsLayerSelectionBorder>
            </MapsLayerSelectionSettings>
        </MapsLayer>
    </MapsLayers>
</SfMaps>

@code {
    public void ShapeSelectedEvent(Syncfusion.Blazor.Maps.ShapeSelectedEventArgs args)
    {
        // Here you can customize your code
    }
}
```

### Double-Click Events

```csharp
<SfMaps>
    <MapsEvents OnDoubleClick="@OnDoubleClickEvent"></MapsEvents>
    <MapsLayers>
        <MapsLayer ShapeData='new {dataOptions= "https://cdn.syncfusion.com/maps/map-data/world-map.json"}' TValue="string">
        </MapsLayer>
    </MapsLayers>
</SfMaps>

@code {
    private void OnDoubleClickEvent(Syncfusion.Blazor.Maps.MouseEventArgs args)
    {
        Console.WriteLine($"Double-clicked at: {args.Latitude}, {args.Longitude}");
    }
}
```

## Zoom and Pan Controls

### Enable/Disable Zoom

```csharp
@using Syncfusion.Blazor.Maps

<SfMaps>
    <MapsZoomSettings Enable="true" EnableSelectionZooming="true" EnablePanning="true">
        <MapsZoomToolbarSettings>
            <MapsZoomToolbarButton ToolbarItems="new List<ToolbarItem>() { ToolbarItem.Zoom, ToolbarItem.ZoomIn, ToolbarItem.ZoomOut,
            ToolbarItem.Pan, ToolbarItem.Reset }"></MapsZoomToolbarButton>
        </MapsZoomToolbarSettings>
    </MapsZoomSettings>
    <MapsLayers>
        <MapsLayer ShapeData='new {dataOptions ="https://cdn.syncfusion.com/maps/map-data/usa.json"}' TValue="string">
        </MapsLayer>
    </MapsLayers>
</SfMaps>
```

### Programmatic Zoom

```csharp
@page "/zoom-control"
@using Syncfusion.Blazor.Maps

<SfMaps>
<MapsZoomSettings Enable="true" ZoomFactor="4"></MapsZoomSettings>
    <MapsLayers>
        <MapsLayer UrlTemplate="https://tile.openstreetmap.org/{level}/{tileX}/{tileY}.png" TValue="string">
        </MapsLayer>
    </MapsLayers>
</SfMaps>
```

## Tooltip and Popup Handling

> **Security Warning:** When binding tooltips or popups to external data sources, always sanitize HTML content to prevent XSS attacks and prompt injection. See [Sanitizing External Content](#sanitizing-external-content) below.

### Basic Tooltip

```csharp
@using Syncfusion.Blazor.Maps

<SfMaps>
    <MapsLayers>
        <MapsLayer ShapeData='new {dataOptions= "https://cdn.syncfusion.com/maps/map-data/world-map.json"}' ShapeDataPath="Name"
                   ShapePropertyPath='new string[] {"name"}' DataSource='PerformanceReport' TValue="Country">
            <MapsLayerTooltipSettings Visible="true" ValuePath="CountryName"
                                  Format="<b>${CountryName}</b><br>Finalist: <b>${Winner}</b><br>Win: <b>${Finalist}">
            </MapsLayerTooltipSettings>
        </MapsLayer>
    </MapsLayers>
</SfMaps>

@code {
    public class Country
    {
        public string Name { get; set; }
        public string Winner { get; set; }
        public string Finalist { get; set; }
        public string CountryName { get; set; }
    }

    public List<Country> PerformanceReport = new List<Country> {
        new Country { CountryName = "India", Name = "India", Finalist = "3", Winner = "2" },
    };
}
```

### Tooltip on Hover

```csharp
@using Syncfusion.Blazor.Maps

<SfMaps>
    <MapsLayers>
        <MapsLayer ShapeData='new {dataOptions= "https://cdn.syncfusion.com/maps/map-data/world-map.json"}' ShapeDataPath="Name"
                   ShapePropertyPath='new string[] {"name"}' DataSource='PerformanceReport' TValue="Country">
            <MapsLayerTooltipSettings Visible="true" ValuePath="CountryName"
                                  Format="<b>${CountryName}</b><br>Finalist: <b>${Winner}</b><br>Win: <b>${Finalist}">
            </MapsLayerTooltipSettings>
            <MapsShapeSettings Fill="#E5E5E5" ColorValuePath="Finalist">
                <MapsShapeColorMappings>
                    <MapsShapeColorMapping Value="1" Color='new string[] {"#acaed8"}'></MapsShapeColorMapping>
                    <MapsShapeColorMapping Value="2" Color='new string[] {"#80c1ff"}'></MapsShapeColorMapping>
                    <MapsShapeColorMapping Value="3" Color='new string[] {"#1a90ff"}'></MapsShapeColorMapping>
                    <MapsShapeColorMapping Value="4" Color='new string[] {"#005cb3"}'></MapsShapeColorMapping>
                    <MapsShapeColorMapping Value="7" Color='new string[] {"#0b0d35"}'></MapsShapeColorMapping>
                </MapsShapeColorMappings>
            </MapsShapeSettings>
        </MapsLayer>
    </MapsLayers>
</SfMaps>

@code {
    public class Country
    {
        public string Name { get; set; }
        public string Winner { get; set; }
        public string Finalist { get; set; }
        public string CountryName { get; set; }
    }

    public List<Country> PerformanceReport = new List<Country> {
        new Country { CountryName = "India", Name = "India", Finalist = "3", Winner = "2" },
        new Country { CountryName = "United Kingdom", Name = "United Kingdom", Finalist = "4", Winner = "1" },
        new Country { CountryName = "Australia", Name = "Australia", Finalist = "7", Winner = "5" },
        new Country { CountryName = "Sri Lanka", Name = "Sri Lanka", Finalist = "3", Winner = "1" },
        new Country { CountryName = "Pakistan", Name = "Pakistan", Finalist = "2", Winner = "1" },
        new Country { CountryName = "New Zealand", Name = "New Zealand", Finalist = "1", Winner = "0" },
        new Country { CountryName = "West Indies", Name = "Dominican Rep", Finalist = "3", Winner = "2" },
        new Country { CountryName = "West Indies", Name = "Cuba", Finalist = "3", Winner = "2" },
        new Country { CountryName = "West Indies", Name = "Jamaica", Finalist = "3", Winner = "2" },
        new Country { CountryName = "West Indies", Name = "Haiti", Finalist = "3", Winner = "2" },
        new Country { CountryName = "West Indies", Name = "Gayana", Finalist = "3", Winner = "2" },
        new Country { CountryName = "West Indies", Name = "Suriname", Finalist = "3", Winner = "2" },
        new Country { CountryName = "West Indies", Name = "Trinidad and Tobago", Finalist = "3", Winner = "2" }
    };
}
```

When user hovers over a shape, the tooltip displays the value from `StateName` property.

### Custom Popup on Click

```csharp
@page "/popup-map"
@using Syncfusion.Blazor.Maps

<SfMaps @ref="mapInstance">
    <MapsEvents OnMarkerClick="MarkerClickHandler" />
    <MapsLayers>
        <MapsLayer ShapeData='new {dataOptions= "https://cdn.syncfusion.com/maps/map-data/world-map.json"}' TValue="string">
            <MapsMarkerSettings>
                <MapsMarker Visible="true" DataSource="MarkerData" Height="25" Width="15" Fill="red" DashArray="1" Opacity="0.9"
                            TValue="City">
                    <MapsMarkerBorder Color="green" Width="2"></MapsMarkerBorder>
                </MapsMarker>
            </MapsMarkerSettings>    
        </MapsLayer>
    </MapsLayers>
</SfMaps>

@if (ShowPopup)
{
    <div style="position: absolute; background: white; border: 1px solid black; padding: 10px; z-index: 1000;">
        <h3>@PopupTitle</h3>
        <p>Latitude: @PopupLat</p>
        <p>Longitude: @PopupLng</p>
        <button @onclick="() => ShowPopup = false">Close</button>
    </div>
}

@code {
    private SfMaps mapInstance;
    private bool ShowPopup = false;
    private string PopupTitle = "";
    private double PopupLat = 0;
    private double PopupLng = 0;

    private void MarkerClickHandler(MarkerClickEventArgs args)
    {
        PopupTitle = "Location Details";
        PopupLat = args.Latitude;
        PopupLng = args.Longitude;
        ShowPopup = true;
    }
    public class City
    {
        public double Latitude { get; set; }
        public double Longitude { get; set; }
    }

    public List<City> MarkerData = new List<City> {
        new City { Latitude = 35.145083, Longitude = -117.960260 },
        new City { Latitude = 40.724546, Longitude = -73.850344 },
        new City { Latitude = 41.657782, Longitude = -91.533857 }
    };
}
```

## Keyboard Navigation

### Keyboard Controls

Enable keyboard navigation for accessibility:

```csharp
<SfMaps>
    <MapsZoomSettings Enable="true"></MapsZoomSettings>
    <MapsLayers>
        <MapsLayer ShapeData='new {dataOptions= "https://cdn.syncfusion.com/maps/map-data/world-map.json"}' TValue="string">
        </MapsLayer>
    </MapsLayers>
</SfMaps>
```

**Keyboard shortcuts:**
- `+` : Zoom in
- `-` : Zoom out
- `Arrow keys`: Pan map
- `Home`: Reset to default view

## Custom Interaction Patterns

### Selection State Management

Track which elements are selected:

```csharp
@page "/selection-tracking"
@using Syncfusion.Blazor.Maps

<div>@MarkerDataSourceList.Count</div>
<SfMaps>
    <MapsEvents OnMarkerClick="MarkerClickHandler" />
    <MapsLayers>
        <MapsLayer ShapeData='new { dataOptions = "https://cdn.syncfusion.com/maps/map-data/world-map.json" }'
                   TValue="string">
            <MapsMarkerSettings>
                <MapsMarker Visible="true"
                            DataSource="@MarkerDataSource"
                            TValue="City">
                </MapsMarker>
            </MapsMarkerSettings>
        </MapsLayer>
    </MapsLayers>
</SfMaps>
@code {
    public List<City> MarkerDataSourceList = new();
    // ✅ Single marker data source
    public List<City> MarkerDataSource = new()
    {
        new City { Latitude = 37.368, Longitude = -122.095 },
        new City { Latitude = 40.724546, Longitude = -73.850344 }
    };

    public class City
    {
        public double Latitude { get; set; }
        public double Longitude { get; set; }
    }

    private void MarkerClickHandler(MarkerClickEventArgs args)
    {
        MarkerDataSourceList.Add(new City
        {
            Latitude = args.Latitude,
            Longitude = args.Longitude
        });
    }
}
```

### Drag to Pan

Most maps support native drag-to-pan. Enable explicitly:

```csharp
@using Syncfusion.Blazor.Maps

<SfMaps>
    <MapsZoomSettings Enable="true" EnablePanning="true" ZoomFactor="2" />
    <MapsLayers>
        <MapsLayer ShapeData='new {dataOptions ="https://cdn.syncfusion.com/maps/map-data/usa.json"}' TValue="string">
        </MapsLayer>
    </MapsLayers>
</SfMaps>
```

Users can now click and drag the map to pan smoothly.

## Sanitizing External Content

> **Critical Security Requirement:** When displaying data from external sources (APIs, user input, databases) in tooltips, popups, or annotations, you MUST sanitize HTML content to prevent XSS attacks and prompt injection vulnerabilities.

### Understanding the Risk

Map components can render HTML content in:
- **Tooltips** - Shown on hover over markers/shapes
- **Annotations** - Custom HTML overlays on the map
- **Popups** - Click-triggered information windows
- **Data labels** - Text displayed on map features

If untrusted data contains malicious HTML/JavaScript, it can:
1. Execute arbitrary JavaScript (XSS attacks)
2. Inject prompt instructions to manipulate AI agents
3. Steal user data or session tokens
4. Redirect users to phishing sites

### Safe Data Binding Patterns

#### Pattern 1: Use Plain Text Only (Recommended)

```csharp
@page "/safe-tooltips"
@using Syncfusion.Blazor.Maps
<SfMaps>
    <MapsLayers>
        <MapsLayer ShapeData='new {dataOptions ="https://cdn.syncfusion.com/maps/map-data/usa.json"}' TValue="string">
            <MapsMarkerSettings>
                <MapsMarker Visible="true" Shape="Syncfusion.Blazor.Maps.MarkerType.Circle" Fill="white" Width="20"
                            DataSource="HighestPopulation" TValue="City">
                    <MapsMarkerBorder Width="2" Color="#333"></MapsMarkerBorder>
                    <MapsMarkerTooltipSettings Visible="true" ValuePath="Name"></MapsMarkerTooltipSettings>
                </MapsMarker>
            </MapsMarkerSettings>
        </MapsLayer>
    </MapsLayers>
</SfMaps>

@code {
    public class City
    {
        public double Latitude { get; set; }
        public double Longitude { get; set; }
        public string Name { get; set; }
    }

    public List<City> HighestPopulation = new List<City> {
        new City { Latitude = 40.7424509, Longitude = -74.0081468, Name = "New York" }
    };
}
```