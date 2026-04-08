# Spatial Features and Overlays

> **MANDATORY SECURITY NOTICE:** This topic includes GeoJSON, tile, and annotation examples for illustration only. In production you MUST host assets locally or serve them via a server-side validation proxy that enforces allow-lists, HTTPS, GeoJSON/schema validation, size/feature limits, property sanitization, and short-lived signed URLs for client consumption. NEVER forward raw `ShapeData`, tooltip, or annotation content to automated agents or LLM prompts without strict validation and documented human review. See [readme-security](readme-security.md) for required validation templates and checklists.

## Table of Contents

- [Polygons and Shapes](#polygons-and-shapes)
   - [Basic Polygon Setup](#basic-polygon-setup)
   - [GeoJSON Format for Polygons](#geojson-format-for-polygons)
   - [Creating Shapes from Data](#creating-shapes-from-data)
   - [Polygon Styling](#polygon-styling)
   - [Polygon with Hover Effects](#polygon-with-hover-effects)
- [Navigation Lines and Polylines](#navigation-lines-and-polylines)
   - [Basic Polyline](#basic-polyline)
   - [Multi-Segment Routes](#multi-segment-routes)
   - [Polyline Styling Options](#polyline-styling-options)
   - [Dynamic Route from Waypoints](#dynamic-route-from-waypoints)
- [Annotations](#annotations)
   - [Text Annotations](#text-annotations)
   - [Icon Annotations](#icon-annotations)
   - [Custom HTML Annotations](#custom-html-annotations)
   - [Dynamic Annotations from Data](#dynamic-annotations-from-data)
- [Bubbles](#bubbles)
   - [Basic Bubbles](#basic-bubbles)
   - [Bubble with Color and Size Mapping](#bubble-with-color-and-size-mapping)
   - [Interactive Bubbles with Tooltips](#interactive-bubbles-with-tooltips)
- [Interactive Spatial Elements](#interactive-spatial-elements)
   - [Clickable Polygons](#clickable-polygons)
   - [Hover Effects on Annotations](#hover-effects-on-annotations)
- [API Reference](#api-reference)
   - [MapsPolygon](#mapspolygon)
   - [MapsPolygons](#mapspolygons)
   - [MapsNavigationLine](#mapsnavigationline)
   - [MapsArrow](#mapsarrow)
   - [MapsAnnotation](#mapsannotation)
   - [MapsAnnotations](#mapsannotations)
   - [MapsBubble<TValue>](#mapsbubbletvalue)
   - [MapsBubbleSettings](#mapsbubblesettings)
   - [Coordinate Class](#coordinate-class)


## Polygons and Shapes

Polygons display geographic boundaries, regions, and areas on the map. They're ideal for showing counties, states, districts, or custom geographic divisions.

### Basic Polygon Setup

```csharp
<SfMaps>
    <MapsLayers>
        <MapsLayer ShapeData="@GeoJsonData" TValue="string">
        </MapsLayer>
    </MapsLayers>
</SfMaps>

@code {
    private object GeoJsonData = // GeoJSON data with polygon features
}
```

### GeoJSON Format for Polygons

GeoJSON is the standard format for geographic data including polygons:

```json
{
  "type": "FeatureCollection",
  "features": [
    {
      "type": "Feature",
      "properties": { "name": "California", "population": 39000000 },
      "geometry": {
        "type": "Polygon",
        "coordinates": [
          [
            [-124.4, 42.0],
            [-120.0, 42.0],
            [-114.6, 35.0],
            [-120.0, 32.5],
            [-124.4, 42.0]
          ]
        ]
      }
    }
  ]
}
```

Each polygon is defined by an array of coordinate pairs [longitude, latitude].

### Creating Shapes from Data

```csharp
@page "/state-map"
@using Syncfusion.Blazor.Maps

<SfMaps>
    <MapsLayers>
        <MapsLayer UrlTemplate="/tiles/{level}/{tileX}/{tileY}.png" TValue="string">
        </MapsLayer>
        <MapsLayer TValue="string"
                   ShapeData='new { dataOptions = "/data/usa-map.json" }'>
                <MapsShapeSettings Fill="rgba(0, 100, 200, 0.3)">
                    <MapsShapeBorder Color="blue" Width="1"></MapsShapeBorder>
                </MapsShapeSettings>
                <MapsDataLabelSettings Visible="true" LabelPath="name">
                </MapsDataLabelSettings>
            </MapsLayer>
    </MapsLayers>
</SfMaps>

@code {
    private object StateData; // Loaded from GeoJSON

    protected override async Task OnInitializedAsync()
    {
        // Load state boundary GeoJSON from API or file
        StateData = await LoadStateGeoJson();
    }

    private async Task<object> LoadStateGeoJson()
    {
        // Implementation to load or return GeoJSON data
        return new { };
    }
}
```

### Polygon Styling

```csharp
<MapsShapeSettings 
    Fill="lightblue"           <!-- Interior color -->
    Stroke="blue"              <!-- Border color -->
    StrokeWidth="2"            <!-- Border width in pixels -->
    Opacity="0.5">             <!-- Transparency (0-1) -->
</MapsShapeSettings>
```

**Color options:**
- Named colors: "red", "blue", "lightblue"
- Hex colors: "#FF0000", "#0000FF"
- RGB: "rgb(255, 0, 0)"
- RGBA with transparency: "rgba(0, 0, 255, 0.5)"

### Polygon with Hover Effects

```csharp
<MapsShapeSettings Fill="lightblue">
</MapsShapeSettings>

<MapsLayerSelectionSettings Enable="true" Fill="green">
    <MapsLayerSelectionBorder Color="white" Width="2"></MapsLayerSelectionBorder>
</MapsLayerSelectionSettings>
```

When user hovers over or selects a polygon, it changes to the selection colors.

## Navigation Lines and Polylines

Polylines connect multiple points to show routes, paths, or connections between locations.

### Basic Polyline

```csharp
<SfMaps>
    <MapsLayers>
        <MapsLayer UrlTemplate="/tiles/{level}/{tileX}/{tileY}.png" TValue="string">
            <MapsNavigationLines>
                <MapsNavigationLine Visible="true" Color="red" Angle="90" Width="3" DashArray="5"
                    Latitude="new double[] { 37.368, 40.7128, 34.0522 }"
                    Longitude="new double[] { -122.095, -74.0060, -118.2437 }">
                </MapsNavigationLine>
            </MapsNavigationLines>
        </MapsLayer>
    </MapsLayers>
</SfMaps>
```

This creates a polyline connecting San Francisco → New York → Los Angeles.

### Multi-Segment Routes

```csharp
<MapsNavigationLines>
    <!-- Route segment 1 -->
    <MapsNavigationLine 
        Latitude="new double[] { 37.368, 40.7128 }"
        Longitude="new double[] { -122.095, -74.0060 }"
        Color="blue" Width="2">
    </MapsNavigationLine>

    <!-- Route segment 2 -->
    <MapsNavigationLine 
        Latitude="new double[] { 40.7128, 34.0522 }"
        Longitude="new double[] { -74.0060, -118.2437 }"
        Color="green" Width="2">
    </MapsNavigationLine>

    <!-- Route segment 3 -->
    <MapsNavigationLine 
        Latitude="new double[] { 34.0522, 37.368 }"
        Longitude="new double[] { -118.2437, -122.095 }"
        Color="red" Width="2">
    </MapsNavigationLine>
</MapsNavigationLines>
```

### Polyline Styling Options

```csharp
<MapsNavigationLine 
    Visible="true"
    Color="blue"
    Angle="90"
    Width="2"
    DashArray="5"
    Latitude="new double[] { }"
    Longitude="new double[] { }">
</MapsNavigationLine>
```

### Dynamic Route from Waypoints

```csharp
@page "/route-builder"
@using Syncfusion.Blazor.Maps

<div style="margin-bottom:10px;">
    <button @onclick="UpdateRoute">Update Route</button>
</div>
<SfMaps @ref="mapInstance">
    <MapsLayers>
        <MapsLayer ShapeData='new {dataOptions= "/data/world-map.json"}'
                   TValue="string">
            <MapsNavigationLines>
                <MapsNavigationLine Visible="true"
                                    Color="red"
                                    Angle="90"
                                    Width="3"
                                    DashArray="5"
                                    Latitude="@RouteLatitudes"
                                    Longitude="@RouteLongitudes">
                </MapsNavigationLine>
            </MapsNavigationLines>
        </MapsLayer>
    </MapsLayers>
</SfMaps>
@code {
    private SfMaps mapInstance;
    // ✅ Bindable coordinate arrays
    private double[] RouteLatitudes = new double[]
    {
        37.368,     // California
        40.7128,    // New York
        34.0522     // Los Angeles
    };

    private double[] RouteLongitudes = new double[]
    {
        -122.095,
        -74.0060,
        -118.2437
    };
    private async Task UpdateRoute()
    {
        // ✅ Update route dynamically
        RouteLatitudes = new double[]
        {
            28.6139,   // New Delhi
            51.5074,   // London
            48.8566    // Paris
        };
        RouteLongitudes = new double[]
        {
            77.2090,
            -0.1278,
            2.3522
        };
    }
}
```

## Annotations

Annotations are text labels, icons, or custom HTML elements placed at specific coordinates on the map. Use them for callouts, labels, or context information.

> **Security Warning:** Annotations render HTML content. When binding to external data sources, always sanitize content to prevent XSS attacks and prompt injection. See [Sanitizing External Content](user-interactions.md#sanitizing-external-content) for detailed guidance.

### Text Annotations

```csharp
@using Syncfusion.Blazor.Maps

<SfMaps>
    <MapsAnnotations>
        <MapsAnnotation X="0%" Y="50%">
            <ContentTemplate>
                <div>
                    Annotation
                </div>
            </ContentTemplate>
        </MapsAnnotation>
    </MapsAnnotations>
    <MapsLayers>
        <MapsLayer ShapeData='new {dataOptions ="/data/world-map.json"}'
                   ShapePropertyPath='new string[] {"name"}' TValue="string">
        </MapsLayer>
    </MapsLayers>
</SfMaps>
```

> **Note:** The above example uses hardcoded content which is safe. When content comes from variables, databases, or APIs, sanitization is required.

### Icon Annotations

```csharp
<MapsAnnotation X="0%" Y="50%">
    <ContentTemplate>
        <div>
            <img style="height: 30px; width: 40px" src='/images/wheel.png'>
        </div>
    </ContentTemplate>
</MapsAnnotation>
```

### Custom HTML Annotations

```csharp
<MapsAnnotation X="0%" Y="50%">
    <Content>
        <div style="background: lightblue; padding: 10px; border-radius: 5px; border: 1px solid blue;">
            <strong>Location Details</strong>
            <p>Latitude: 37.368</p>
            <p>Longitude: -122.095</p>
        </div>
    </Content>
</MapsAnnotation>
```

### Dynamic Annotations from Data

> **Security Critical:** This example demonstrates UNSAFE binding. See secure version below.

```csharp
@page "/annotated-map"
@using Syncfusion.Blazor.Maps

<div style="margin-bottom:10px;">
    <button @onclick="UpdateAnnotation">Update Annotation</button>
</div>

<SfMaps>
    <MapsAnnotations>
        <MapsAnnotation X="@X" Y="@Y">
            <Content>
                <div style="background: lightblue; padding: 5px; border-radius: 5px; border: 1px solid blue;">
                    <strong>Location Details</strong>
                    <p>Latitude: @Latitude</p>
                    <p>Longitude: @Longitude</p>
                </div>
            </Content>
        </MapsAnnotation>
    </MapsAnnotations>

    <MapsLayers>
        <MapsLayer ShapeData='new { dataOptions = "https://cdn.syncfusion.com/maps/map-data/world-map.json" }'
                   ShapePropertyPath='new string[] { "name" }'
                   TValue="string">
        </MapsLayer>
    </MapsLayers>
</SfMaps>
@code {
    private double Latitude = 37.368;
    private double Longitude = -122.095;

    // Annotation position
    private string X = "0%";
    private string Y = "50%";

    private void UpdateAnnotation()
    {
        // ✅ Dynamically update values
        Latitude = 28.6139;     // New Delhi
        Longitude = 77.2090;

        // ✅ Move annotation position
        X = "50%";
        Y = "30%";

        // ✅ Re-render annotation content
        StateHasChanged();
    }
}
```

#### Secure Version with HTML Encoding

```csharp
@page "/annotated-map-secure"
@using Syncfusion.Blazor.Maps
@using System.Text.Encodings.Web

<div style="margin-bottom:10px;">
    <button @onclick="UpdateAnnotation">Update Annotation</button>
</div>
<SfMaps>
    <MapsAnnotations>
        <MapsAnnotation X="@X" Y="@Y">
            <Content>
                <div style="background: lightblue; padding: 5px; border-radius: 5px; border: 1px solid blue;">
                    <strong>Location Details</strong>
                    <p>Latitude: @Encode(Latitude)</p>
                    <p>Longitude: @Encode(Longitude)</p>
                </div>
            </Content>
        </MapsAnnotation>
    </MapsAnnotations>
    <MapsLayers>
        <MapsLayer ShapeData='new { dataOptions = "https://cdn.syncfusion.com/maps/map-data/world-map.json" }'
                   ShapePropertyPath='new string[] { "name" }'
                   TValue="string">
        </MapsLayer>
    </MapsLayers>
</SfMaps>
@code {
    private double Latitude = 37.368;
    private double Longitude = -122.095;
    private string X = "0%";
    private string Y = "50%";

    // ✅ Centralized HTML Encoder
    private static readonly HtmlEncoder Encoder = HtmlEncoder.Default;

    // ✅ Encoding helper
    private string Encode(object value)
    {
        return Encoder.Encode(value?.ToString() ?? string.Empty);
    }

    private void UpdateAnnotation()
    {
        // Example dynamic update (could come from API / user input)
        Latitude = 28.6139;
        Longitude = 77.2090;

        X = "50%";
        Y = "30%";

        // Re-render annotation safely
        StateHasChanged();
    }
}
``
```

> **Required Package:** `System.Web.HttpUtility` is available in `System.Web` (or use `Microsoft.AspNetCore.WebUtilities.HtmlEncoder` in modern .NET)

## Bubbles

Bubbles are circular overlays that represent data values through their size. They're useful for showing proportional data like population, sales, or other metrics at specific locations.

⚠️ **IMPORTANT:** Use `<MapsBubble<TValue>>` component (not `<MapsBubbles>` container).

### Basic Bubbles

```csharp
@using Syncfusion.Blazor.Maps

<SfMaps>
    <MapsLayers>
        <MapsLayer ShapeData='new {dataOptions ="https://cdn.syncfusion.com/maps/map-data/world-map.json"}'
                   DataSource="PopulationDetails" ShapeDataPath="Name" ShapePropertyPath='new string[] {"name"}' TValue="Country">
            <MapsBubbleSettings>
                <MapsBubble Visible="true" ValuePath="Population" DataSource="PopulationDetails" TValue="Country">
                </MapsBubble>
            </MapsBubbleSettings>
        </MapsLayer>
    </MapsLayers>
</SfMaps>

@code {
    public class Country
    {
        public string Name { get; set; }
        public double Population { get; set; }
    };

    public List<Country> PopulationDetails = new List<Country> {
        new Country { Name = "United States", Population = 325020000 }
    };
}
```

### Bubble with Color and Size Mapping

```csharp
<MapsBubbleSettings>
    <MapsBubble Visible="true"
                DataSource="@BubbleData"
                TValue="BubbleDataClass"
                ValuePath="Population"
                MinRadius="10" MaxRadius="40"
                ColorValuePath="GrowthRate"
                Fill="blue" Opacity="0.6">
    </MapsBubble>
</MapsBubbleSettings>
```

Bubbles automatically size proportionally to the `ValuePath` field and color based on `ColorValuePath` values.

### Interactive Bubbles with Tooltips

```csharp
<MapsBubbleSettings>
    <MapsBubble Visible="true"
                DataSource="@CityDataSource"
                TValue="CityData"
                ValuePath="Population"
                MinRadius="10" MaxRadius="40"
                Fill="orange" Opacity="0.5">
        <MapsBubbleTooltipSettings Visible="true" 
                                   ValuePath="Name">
        </MapsBubbleTooltipSettings>
    </MapsBubble>
</MapsBubbleSettings>
```

## Interactive Spatial Elements

### Clickable Polygons

```csharp
@page "/interactive-shapes"
@using Syncfusion.Blazor.Maps

<SfMaps @ref="mapInstance">
    <MapsEvents ShapeSelected="ShapeSelected" />
    <MapsLayers>
        <MapsLayer ShapeData='new {dataOptions= "/data/world-map.json"}' TValue="string">
            <MapsShapeSettings Fill="lightblue"/>
                 <MapsLayerSelectionSettings Enable="true" Fill="green">
                    <MapsLayerSelectionBorder Color="white" Width="2"></MapsLayerSelectionBorder>
                </MapsLayerSelectionSettings>
        </MapsLayer>
    </MapsLayers>
</SfMaps>

<p>@SelectedInfo</p>

@code {
    private SfMaps mapInstance;
    private object StateData;
    private string SelectedInfo = "Click a state to see details";

    private void ShapeSelected(ShapeSelectedEventArgs args)
    {
        SelectedInfo = $"Selected: {args.ShapeData}";
    }
}
```

## API Reference

### MapsPolygon

Defines a polygon shape on the map.

```csharp
<MapsPolygons>
    <MapsPolygon Fill="transparent" BorderColor="red" Points="@Coordinates" TooltipText="Line String" BorderWidth="2" ShapeType="PolygonShapeType.Polygon">
    </MapsPolygon>
    <MapsPolygonTooltipSettings Visible="true" BorderColor="Red" BorderWidth="1">
    </MapsPolygonTooltipSettings>
    <MapsPolygonHighlightSettings Enable=true Fill="yellow" Opacity="0.4">
        <MapsPolygonHighlightBorder Color="blue" Opacity="1" Width="1"></MapsPolygonHighlightBorder>
    </MapsPolygonHighlightSettings>
    <MapsPolygonSelectionSettings Enable=true EnableMultiSelect=false Fill="violet" Opacity="0.8">
        <MapsPolygonSelectionBorder Color="cyan" Opacity="1" Width="1"></MapsPolygonSelectionBorder>
    </MapsPolygonSelectionSettings>
</MapsPolygons>
```

| Property | Type | Description |
|----------|------|-------------|
| `PolygonType` | `PolygonShapeType` | Shape: LineString, Polygon |
| `Points` | `Coordinate[]` | Array of lat/lng points forming polygon |
| `Fill` | string | Fill color (hex, rgb, or named) |
| `Opacity` | double | Transparency (0-1) |

### MapsPolygons

Container for multiple polygon shapes.

```csharp
<MapsPolygons>
    <MapsPolygon Points="@polygon1Points" Fill="blue" Opacity="0.5">
    </MapsPolygon>
    <MapsPolygon Points="@polygon2Points" Fill="red" Opacity="0.5">
    </MapsPolygon>
</MapsPolygons>
```

### MapsNavigationLine

Defines a line connecting two or more locations.

```csharp
<MapsNavigationLines>
    <MapsNavigationLine Visible="true" Color="blue" Angle="90" Width="2" DashArray="4"
                        Latitude="new double[]{ 40.7128, 36.7783 }" Longitude="new double[]{ -74.0060, -119.4179 }">
        <MapsArrow ShowArrow="true" Color="blue"></MapsArrow>
        <MapsNavigationLineHighlightSettings Fill="green" Enable="true"></MapsNavigationLineHighlightSettings>
        <MapsNavigationLineSelectionSettings Fill="blue" Enable="true"></MapsNavigationLineSelectionSettings>
    </MapsNavigationLine>
</MapsNavigationLines>
```

| Property | Type | Description |
|----------|------|-------------|
| `Coordinates` | `Coordinate[]` | Array of points forming line |
| `Stroke` | string | Line color |
| `StrokeWidth` | double | Line thickness |
| `Fill` | string | Line fill color |
| `Opacity` | double | Transparency (0-1) |

### MapsArrow

Defines arrow head on navigation lines.

```csharp
<MapsArrow Position="End"
           Width="10"
           Height="10"
           Fill="blue"
           Opacity="0.8">
</MapsArrow>
```

| Property | Type | Description |
|----------|------|-------------|
| `Position` | string | Start, End |
| `Width` | double | Arrow width in pixels |
| `Height` | double | Arrow height in pixels |
| `Fill` | string | Arrow color |
| `Opacity` | double | Transparency |

### MapsAnnotation

Defines text, image, or shape annotations.

```csharp
<MapsAnnotations>
    <MapsAnnotation X="0%" Y="10%"
                VerticalAlignment="Syncfusion.Blazor.Maps.AnnotationAlignment.Center" 
                HorizontalAlignment="Syncfusion.Blazor.Maps.AnnotationAlignment.Center">
        <ContentTemplate>
                <div>
                    <div id="first"><h1>Maps</h1></div>
                </div>
            </ContentTemplate>
    </MapsAnnotation>
</MapsAnnotations>
```

| Property | Type | Description |
|----------|------|-------------|
| `ContentTemplate` | Tag | Text, HTML, or template content |
| `X` | double | Latitude coordinate |
| `Y` | double | Longitude coordinate |
| `Alignment` | `AnnotationAlignment` | Horizontal alignment |
| `VerticalAlignment` | double | Vertical alignment |
| `ZIndex` | double | Layer depth |

### MapsAnnotations

Container for multiple annotations.

```csharp
<MapsAnnotations>
    <MapsAnnotation X="0%" Y="10%"
                VerticalAlignment="Syncfusion.Blazor.Maps.AnnotationAlignment.Center" 
                HorizontalAlignment="Syncfusion.Blazor.Maps.AnnotationAlignment.Center">
        <ContentTemplate>
                <div>
                    <div id="first"><h1>Maps</h1></div>
                </div>
            </ContentTemplate>
    </MapsAnnotation>
    <MapsAnnotation X="0%" Y="30%"
                VerticalAlignment="Syncfusion.Blazor.Maps.AnnotationAlignment.Center" 
                HorizontalAlignment="Syncfusion.Blazor.Maps.AnnotationAlignment.Center">
        <ContentTemplate>
                <div>
                    <div id="first"><h1>Annotation</h1></div>
                </div>
            </ContentTemplate>
    </MapsAnnotation>
</MapsAnnotations>
```

### MapsBubble<TValue>

Defines data bubbles on the map. **Always requires `TValue` generic parameter.**

```csharp
<MapsBubbleSettings>
    <MapsBubble Visible="true"
                DataSource="@BubbleData"
                TValue="MyBubbleClass"
                ValuePath="DataValue"
                Fill="blue"
                Opacity="0.7">
    </MapsBubble>
</MapsBubbleSettings>

@code {
    public class MyBubbleClass 
    {
        public double Latitude { get; set; }
        public double Longitude { get; set; }
        public double DataValue { get; set; }
    }
}
```

| Property | Type | Description |
|----------|------|-------------|
| `DataSource` | IEnumerable<object> | Data collection for bubbles |
| `TValue` | Generic | Data model type (REQUIRED) |
| `ValuePath` | string | Data field affecting bubble size |
| `ColorValuePath` | string | Data field affecting bubble color |
| `Fill` | string | Bubble color |
| `Opacity` | double | Transparency (0-1) |
| `MinRadius` | double | Minimum bubble size |
| `MaxRadius` | double | Maximum bubble size |

### MapsBubbleSettings

Configure default bubble appearance.

```csharp
@using Syncfusion.Blazor.Maps

<SfMaps>
    <MapsLayers>
        <MapsLayer ShapeData='new {dataOptions ="/data/world-map.json"}'
                   DataSource="PopulationDetails" ShapeDataPath="Name" ShapePropertyPath='new string[] {"name"}' TValue="Country">
            <MapsBubbleSettings>
                <MapsBubble MinRadius="5" MaxRadius="25" Visible="true" ValuePath="Population" ColorValuePath="Color" DataSource="PopulationDetails" TValue="Country">
                </MapsBubble>
            </MapsBubbleSettings>
        </MapsLayer>
    </MapsLayers>
</SfMaps>

@code {
    public class Country
    {
        public string Name { get; set; }
        public double Population { get; set; }
        public string Color { get; set; }
    };

    public List<Country> PopulationDetails = new List<Country> {
        new Country { Name = "United States", Population = 325020000, Color = "#b5e485" },
        new Country { Name = "Russia", Population = 142905208, Color = "#7bc1e8" },
       new Country { Name ="India", Population=1198003000, Color = "#df819c" }
    };
}
```

| Property | Type | Description |
|----------|------|-------------|
| `MinRadius` | double | Minimum bubble radius |
| `MaxRadius` | double | Maximum bubble radius |
| `ValuePath` | string | Data field for bubble size |
| `ColorValuePath` | string | Data field for color |
