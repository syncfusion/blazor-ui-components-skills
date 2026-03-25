# Spatial Features and Overlays

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
        <MapsLayer UrlTemplate="https://tile.openstreetmap.org/level/tileX/tileY.png" TValue="string">
        </MapsLayer>
        <MapsLayer TValue="string">
            <MapsLayerShapeSettings DataSource="@GeoJsonData">
                <MapsShapeSettings Fill="lightblue">
                    <MapsShapeBorder Color="blue" Width="1"></MapsShapeBorder>
                </MapsShapeSettings>
            </MapsLayerShapeSettings>
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

<SfMaps >
<MapsZoomSettings ZoomFactor="4"></MapsZoomSettings>
    <MapsLayers>
        <MapsLayer UrlTemplate="https://tile.openstreetmap.org/level/tileX/tileY.png" TValue="string">
        </MapsLayer>
        <MapsLayer TValue="string"
                   ShapeData='new { dataOptions = "https://cdn.syncfusion.com/maps/map-data/usa.json" }'
                   DataSource="@StateData"
                   ShapePropertyPath='new string[] { "name" }'>
                <MapsShapeSettings Fill="rgba(0, 100, 200, 0.3)">
                    <MapsShapeBorder Color="blue" Width="1"></MapsShapeBorder>
                </MapsShapeSettings>
                <MapsDataLabelSettings Visible="true" LabelPath="name">
                </MapsDataLabelSettings>
            </MapsLayer>
    </MapsLayers>
</SfMaps>

@code {
    private int ZoomLevel = 4;
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
<MapsShapeSettings Fill="lightblue" Stroke="blue" StrokeWidth="1">
</MapsShapeSettings>

<MapsShapeSelectionSettings Enable="true" 
    Fill="orange" Stroke="red" StrokeWidth="2">
</MapsShapeSelectionSettings>
```

When user hovers over or selects a polygon, it changes to the selection colors.

## Navigation Lines and Polylines

Polylines connect multiple points to show routes, paths, or connections between locations.

### Basic Polyline

```csharp
<SfMaps >
<MapsZoomSettings ZoomFactor="4"></MapsZoomSettings>
    <MapsLayers>
        <MapsLayer UrlTemplate="https://tile.openstreetmap.org/level/tileX/tileY.png" TValue="string">
            <MapsNavigationLines>
                <MapsNavigationLine Visible="true" Color="red" Angle="90" Width="3" DashArray="5"
                    Latitude="@new double[] { 37.368, 40.7128, 34.0522 }"
                    Longitude="@new double[] { -122.095, -74.0060, -118.2437 }">
                </MapsNavigationLine>
            </MapsNavigationLines>
        </MapsLayer>
    </MapsLayers>
</SfMaps>

@code {
    private int ZoomLevel = 4;
}
```

This creates a polyline connecting San Francisco → New York → Los Angeles.

### Multi-Segment Routes

```csharp
<MapsNavigationLines>
    <!-- Route segment 1 -->
    <MapsNavigationLine 
        Latitude="@new double[] { 37.368, 40.7128 }"
        Longitude="@new double[] { -122.095, -74.0060 }"
        Color="blue" Width="2">
    </MapsNavigationLine>

    <!-- Route segment 2 -->
    <MapsNavigationLine 
        Latitude="@new double[] { 40.7128, 34.0522 }"
        Longitude="@new double[] { -74.0060, -118.2437 }"
        Color="green" Width="2">
    </MapsNavigationLine>

    <!-- Route segment 3 -->
    <MapsNavigationLine 
        Latitude="@new double[] { 34.0522, 37.368 }"
        Longitude="@new double[] { -118.2437, -122.095 }"
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
    Latitude="@new double[] { }"
    Longitude="@new double[] { }">
</MapsNavigationLine>
```

### Dynamic Route from Waypoints

```csharp
@page "/route-builder"
@using Syncfusion.Blazor.Maps

<button @onclick="AddWaypoint">Add Waypoint</button>
<button @onclick="ClearRoute">Clear Route</button>

<div style="width: 100%; height: 500px;">
    <SfMaps @ref="mapInstance" >
    <MapsZoomSettings ZoomFactor="4"></MapsZoomSettings>
        <MapsLayers>
            <MapsLayer UrlTemplate="https://tile.openstreetmap.org/level/tileX/tileY.png" TValue="string">
                <!-- Polyline connecting waypoints -->
                @if (Waypoints.Count > 1)
                {
                    <MapsNavigationLine 
                        Latitude="@Waypoints.Select(w => w.Latitude).ToArray()"
                        Longitude="@Waypoints.Select(w => w.Longitude).ToArray()"
                        Color="blue" Width="3">
                    </MapsNavigationLine>
                }

                <!-- Waypoint markers -->
                <MapsMarkerSettings>
                    @foreach (var wp in Waypoints)
                    {
                        <MapsMarker Latitude="@wp.Latitude" Longitude="@wp.Longitude"
                            Shape="MarkerType.Circle" Width="12" Height="12"
                            Fill="green">
                        </MapsMarker>
                    }
                </MapsMarkerSettings>
            </MapsLayer>
        </MapsLayers>
    </SfMaps>
</div>

@code {
    private SfMaps mapInstance;
    private MapsCenterPosition CenterLatLng = new MapsCenterPosition { Latitude = 39.0, Longitude = -98.0 };
    private List<WaypointData> Waypoints = new();
    private Random random = new();

    private async Task AddWaypoint()
    {
        Waypoints.Add(new WaypointData 
        { 
            Latitude = 25 + (random.NextDouble() * 50), 
            Longitude = -125 + (random.NextDouble() * 50) 
        });
        await mapInstance.RefreshAsync();
    }

    private async Task ClearRoute()
    {
        Waypoints.Clear();
        await mapInstance.RefreshAsync();
    }

    public class WaypointData
    {
        public double Latitude { get; set; }
        public double Longitude { get; set; }
    }
}
```

## Annotations

Annotations are text labels, icons, or custom HTML elements placed at specific coordinates on the map. Use them for callouts, labels, or context information.

### Text Annotations

```csharp
<SfMaps>
    <MapsLayers>
        <MapsLayer UrlTemplate="https://tile.openstreetmap.org/level/tileX/tileY.png" TValue="string">
            <MapsAnnotations>
                <MapsAnnotation Latitude="37.368" Longitude="-122.095"
                    Content="<div style='background: white; padding: 5px; border-radius: 3px;'>San Francisco</div>">
                </MapsAnnotation>
                <MapsAnnotation Latitude="40.7128" Longitude="-74.0060"
                    Content="<div style='background: white; padding: 5px; border-radius: 3px;'>New York</div>">
                </MapsAnnotation>
            </MapsAnnotations>
        </MapsLayer>
    </MapsLayers>
</SfMaps>
```

### Icon Annotations

```csharp
<MapsAnnotation Latitude="37.368" Longitude="-122.095"
    Content="<img src='/images/info-icon.png' width='24' height='24' />">
</MapsAnnotation>
```

### Custom HTML Annotations

```csharp
<MapsAnnotation Latitude="37.368" Longitude="-122.095">
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

```csharp
@page "/annotated-map"
@using Syncfusion.Blazor.Maps

<SfMaps >
<MapsZoomSettings ZoomFactor="4"></MapsZoomSettings>
    <MapsLayers>
        <MapsLayer UrlTemplate="https://tile.openstreetmap.org/level/tileX/tileY.png" TValue="string">
            <MapsAnnotations>
                @foreach (var location in Locations)
                {
                    <MapsAnnotation Latitude="@location.Latitude" 
                        Longitude="@location.Longitude"
                        Content="@($"<div style='background: white; padding: 5px; font-weight: bold;'>{location.Name}</div>")">
                    </MapsAnnotation>
                }
            </MapsAnnotations>
        </MapsLayer>
    </MapsLayers>
</SfMaps>

@code {
    private MapsCenterPosition CenterLatLng = new MapsCenterPosition { Latitude = 39.0, Longitude = -98.0 };

    private List<LocationData> Locations = new()
    {
        new LocationData { Latitude = 37.368, Longitude = -122.095, Name = "San Francisco" },
        new LocationData { Latitude = 40.7128, Longitude = -74.0060, Name = "New York" },
        new LocationData { Latitude = 34.0522, Longitude = -118.2437, Name = "Los Angeles" }
    };

    public class LocationData
    {
        public double Latitude { get; set; }
        public double Longitude { get; set; }
        public string Name { get; set; }
    }
}
```

## Bubbles

Bubbles are circular overlays that represent data values through their size. They're useful for showing proportional data like population, sales, or other metrics at specific locations.

⚠️ **IMPORTANT:** Use `<MapsBubble<TValue>>` component (not `<MapsBubbles>` container).

### Basic Bubbles

```csharp
<SfMaps>
    <MapsLayers>
        <MapsLayer TValue="CityData" UrlTemplate="https://tile.openstreetmap.org/level/tileX/tileY.png">
            <MapsBubbleSettings>
                <!-- Use MapsBubble component with TValue - NOT MapsBubbles container -->
                <MapsBubble Visible="true"
                            DataSource="@CityData"
                            TValue="CityData"
                            LatitudeValuePath="Latitude"
                            LongitudeValuePath="Longitude"
                            ValuePath="Population"
                            MinRadius="5" MaxRadius="50"
                            Fill="blue" Opacity="0.5">
                </MapsBubble>
            </MapsBubbleSettings>
        </MapsLayer>
    </MapsLayers>
</SfMaps>

@code {
    private List<CityData> CityData = new()
    {
        new CityData { Latitude = 37.368, Longitude = -122.095, Population = 870000 },
        new CityData { Latitude = 40.7128, Longitude = -74.0060, Population = 8300000 },
        new CityData { Latitude = 34.0522, Longitude = -118.2437, Population = 3900000 }
    };

    public class CityData
    {
        public double Latitude { get; set; }
        public double Longitude { get; set; }
        public int Population { get; set; }
    }
}
```

### Bubble with Color and Size Mapping

```csharp
<MapsBubbleSettings>
    <MapsBubble Visible="true"
                DataSource="@BubbleData"
                TValue="BubbleDataClass"
                LatitudeValuePath="Latitude"
                LongitudeValuePath="Longitude"
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
                DataSource="@CityData"
                TValue="CityData"
                LatitudeValuePath="Latitude"
                LongitudeValuePath="Longitude"
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

<SfMaps @ref="mapInstance" OnShapeSelected="ShapeSelected">
    <MapsLayers>
        <MapsLayer UrlTemplate="https://tile.openstreetmap.org/level/tileX/tileY.png" TValue="string">
        </MapsLayer>
        <MapsLayer TValue="string">
            <MapsLayerShapeSettings DataSource="@StateData">
                <MapsShapeSettings Fill="lightblue" Stroke="blue" StrokeWidth="1">
                </MapsShapeSettings>
                <MapsShapeSelectionSettings Enable="true" Fill="orange" Stroke="red">
                </MapsShapeSelectionSettings>
            </MapsLayerShapeSettings>
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

### Hover Effects on Annotations

```csharp
<div @onmouseenter="() => HoverAnnotation = true" 
     @onmouseleave="() => HoverAnnotation = false"
     style="@(HoverAnnotation ? "background: yellow;" : "background: white;")">
    Important Location
</div>

@code {
    private bool HoverAnnotation = false;
}
```

## API Reference

### MapsPolygon

Defines a polygon shape on the map.

```csharp
<MapsPolygon PolygonType="PolygonShapeType.Polygon"
             Points="@polygonPoints"
             Fill="rgba(0, 0, 255, 0.3)"
             Opacity="0.7">
    <MapsPolygonHighlightSettings Enable="true"
                                  Fill="yellow"
                                  Opacity="0.5">
        <MapsPolygonHighlightBorder Color="orange" Width="2">
        </MapsPolygonHighlightBorder>
    </MapsPolygonHighlightSettings>
    <MapsPolygonSelectionSettings Enable="true"
                                  Fill="cyan"
                                  Opacity="0.7">
        <MapsPolygonSelectionBorder Color="blue" Width="3">
        </MapsPolygonSelectionBorder>
    </MapsPolygonSelectionSettings>
</MapsPolygon>
```

| Property | Type | Description |
|----------|------|-------------|
| `PolygonType` | `PolygonShapeType` | Shape: Circle, Rectangle, Triangle, Polygon |
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
<MapsNavigationLine Coordinates="@lineCoordinates"
                    Fill="blue"
                    Stroke="blue"
                    StrokeWidth="2"
                    Opacity="0.8">
    <MapsArrow Position="ArrowPosition.End"
               Width="10"
               Height="10"
               Fill="blue">
    </MapsArrow>
    <!-- Highlight settings with border customization -->
    <MapsNavigationLineHighlightSettings Enable="true"
                                         Fill="yellow"
                                         Opacity="0.6">
        <MapsNavigationLineHighlightBorder Color="orange" Width="2">
        </MapsNavigationLineHighlightBorder>
    </MapsNavigationLineHighlightSettings>
    
    <!-- Selection settings with border customization (NEW - Previously Missing) -->
    <MapsNavigationLineSelectionSettings Enable="true" 
                                         Fill="cyan"
                                         Opacity="0.7">
        <MapsNavigationLineSelectionBorder Color="blue" Width="3">
        </MapsNavigationLineSelectionBorder>
    </MapsNavigationLineSelectionSettings>
</MapsNavigationLine>
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
<MapsArrow Position="ArrowPosition.End"
           Width="10"
           Height="10"
           Fill="blue"
           Opacity="0.8">
</MapsArrow>
```

| Property | Type | Description |
|----------|------|-------------|
| `Position` | `ArrowPosition` | Start, End, or Both |
| `Width` | double | Arrow width in pixels |
| `Height` | double | Arrow height in pixels |
| `Fill` | string | Arrow color |
| `Opacity` | double | Transparency |

### MapsAnnotation

Defines text, image, or shape annotations.

```csharp
<MapsAnnotation Content="Important Location"
                X="37.368"
                Y="-122.095"
                Alignment="AnnotationAlignment.Center"
                VerticalAlignment="VerticalAlignment.Middle">
</MapsAnnotation>
```

| Property | Type | Description |
|----------|------|-------------|
| `Content` | string | Text, HTML, or template content |
| `X` | double | Latitude coordinate |
| `Y` | double | Longitude coordinate |
| `Alignment` | `AnnotationAlignment` | Horizontal alignment |
| `VerticalAlignment` | double | Vertical alignment |
| `ZIndex` | double | Layer depth |

### MapsAnnotations

Container for multiple annotations.

```csharp
<MapsAnnotations>
    <MapsAnnotation Content="Location 1" X="37.368" Y="-122.095">
    </MapsAnnotation>
    <MapsAnnotation Content="Location 2" X="40.7128" Y="-74.0060">
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
                LatitudeValuePath="Latitude"
                LongitudeValuePath="Longitude"
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
| `LatitudeValuePath` | string | Data field for latitude |
| `LongitudeValuePath` | string | Data field for longitude |
| `ValuePath` | string | Data field affecting bubble size |
| `ColorValuePath` | string | Data field affecting bubble color |
| `Fill` | string | Bubble color |
| `Opacity` | double | Transparency (0-1) |
| `MinRadius` | double | Minimum bubble size |
| `MaxRadius` | double | Maximum bubble size |

### MapsBubbleSettings

Configure default bubble appearance.

```csharp
<MapsBubbleSettings MinRadius="5"
                    MaxRadius="25"
                    ValuePath="sales"
                    ColorValuePath="revenue"
                    Fill="blue"
                    Opacity="0.7">
    <MapsBubbleColorMappings>
        <MapsBubbleColorMapping StartRange="0" EndRange="100000" Color='new string[] { "#B3E5FC" }'>
        </MapsBubbleColorMapping>
    </MapsBubbleColorMappings>
</MapsBubbleSettings>
```

| Property | Type | Description |
|----------|------|-------------|
| `MinRadius` | double | Minimum bubble radius |
| `MaxRadius` | double | Maximum bubble radius |
| `ValuePath` | string | Data field for bubble size |
| `ColorValuePath` | string | Data field for color |

### Coordinate Class

Represents a latitude/longitude point.

```csharp
public class Coordinate
{
    public double Latitude { get; set; }   // -90 to 90
    public double Longitude { get; set; }  // -180 to 180
}

// Usage
var point = new Coordinate { Latitude = 37.368, Longitude = -122.095 };
var coordinates = new Coordinate[]
{
    new() { Latitude = 37.368, Longitude = -122.095 },
    new() { Latitude = 40.7128, Longitude = -74.0060 },
    new() { Latitude = 34.0522, Longitude = -118.2437 }
};
```


