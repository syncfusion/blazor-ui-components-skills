# Markers and Layers

## Table of Contents

- [Understanding Markers](#understanding-markers)
- [Adding Markers](#adding-markers)
   - [Single Marker](#single-marker)
   - [Multiple Markers from Data Collection](#multiple-markers-from-data-collection)
- [Marker Types and Styling](#marker-types-and-styling)
   - [Marker Shape Types](#marker-shape-types)
   - [Marker Styling](#marker-styling)
   - [Image Markers](#image-markers)
- [Marker Clustering](#marker-clustering)
- [Understanding Layers](#understanding-layers)
- [Working with Multiple Layers](#working-with-multiple-layers)
   - [Stacking Layers](#stacking-layers)
   - [Independent Layer Styling](#independent-layer-styling)
- [Layer Visibility and Management](#layer-visibility-and-management)
   - [Toggle Layer Visibility](#toggle-layer-visibility)
- [Dynamic Marker Updates](#dynamic-marker-updates)
   - [Adding Markers Programmatically](#adding-markers-programmatically)
   - [Updating Existing Markers](#updating-existing-markers)
   - [Real-Time Marker Updates](#real-time-marker-updates)
- [Performance Optimization](#performance-optimization)
- [MapsMarker<TValue> API Reference](#mapsmarkertvalue-api-reference)
   - [Key Properties](#key-properties)
- [MapsLayer<TValue> API Reference](#mapslayertvalue-api-reference)
   - [Key Properties](#key-properties)
- [MapsMarkerClusterSettings API](#mapsmarkerclustersettings-api)
- [MapsMarkerSettings API](#mapsmarkersettings-api)
- [MapsShapeSettings API](#mapsshapesettings-api)


## Understanding Markers

Markers are point-based annotations placed on the map at specific coordinates. They represent locations, points of interest, or data points and are the fundamental building blocks of location-based applications.

**Marker properties:**
- **Latitude:** Y-coordinate (-90 to 90)
- **Longitude:** X-coordinate (-180 to 180)
- **MarkerType:** Visual representation (Circle, Rectangle, Triangle, etc.)
- **Width/Height:** Dimensions in pixels
- **Color:** Visual styling
- **Tooltip:** Hover information
- **Popup:** Click information

## Adding Markers

### Single Marker

⚠️ **IMPORTANT:** Always specify `TValue` on generic components (`MapsLayer`, `MapsMarker`).

```csharp
@using Syncfusion.Blazor.Maps

<SfMaps>
    <MapsLayers>
        <MapsLayer ShapeData='new { dataOptions = "https://cdn.syncfusion.com/maps/map-data/world-map.json" }'
                   TValue="CityData">
            <MapsMarkerSettings>
                <MapsMarker DataSource="@MarkerData" 
                            TValue="CityData"
                            LatitudeValuePath="Latitude"
                            LongitudeValuePath="Longitude"
                            Shape="MarkerType.Circle" Width="20" Height="20"
                            Fill="red">
                    <MapsMarkerTooltipSettings Visible="true" ValuePath="Name"></MapsMarkerTooltipSettings>
                </MapsMarker>
            </MapsMarkerSettings>
        </MapsLayer>
    </MapsLayers>
</SfMaps>

@code {
    public class CityData
    {
        public double Latitude { get; set; }
        public double Longitude { get; set; }
        public string Name { get; set; }
    }

    private List<CityData> MarkerData = new()
    {
        new CityData { Latitude = 37.368, Longitude = -122.095, Name = "San Francisco" }
    };
}
```

### Multiple Markers from Data Collection

Bind multiple markers to a data source. The recommended approach is to use **`MapsMarkerSettings`** with a **`MapsMarker<TValue>` component** and **`DataSource`**:

```csharp
@using Syncfusion.Blazor.Maps

<SfMaps>
    <MapsLayers>
        <MapsLayer ShapeData='new { dataOptions = "https://cdn.syncfusion.com/maps/map-data/world-map.json" }' 
                   TValue="LocationData">
            <MapsMarkerSettings>
                <!-- Use MapsMarker with TValue - NOT MapsMarkers container -->
                <MapsMarker DataSource="@Locations" 
                            TValue="LocationData"
                            LatitudeValuePath="Latitude"
                            LongitudeValuePath="Longitude"
                            ColorValuePath="Color"
                            Shape="MarkerType.Circle" Width="15" Height="15">
                    <MapsMarkerTooltipSettings Visible="true" ValuePath="Name"></MapsMarkerTooltipSettings>
                </MapsMarker>
            </MapsMarkerSettings>
        </MapsLayer>
    </MapsLayers>
</SfMaps>

@code {
    public class LocationData
    {
        public double Latitude { get; set; }
        public double Longitude { get; set; }
        public string Name { get; set; }
        public string Color { get; set; }
    }

    private List<LocationData> Locations = new()
    {
        new LocationData { Latitude = 37.368, Longitude = -122.095, Name = "San Francisco", Color = "blue" },
        new LocationData { Latitude = 40.7128, Longitude = -74.0060, Name = "New York", Color = "green" },
        new LocationData { Latitude = 34.0522, Longitude = -118.2437, Name = "Los Angeles", Color = "red" }
    };
}
```

**Key Points:**
- ✅ Use `<MapsMarkerSettings>` container
- ✅ Use `<MapsMarker TValue="LocationData">` component with TValue
- ❌ Do NOT use `<MapsMarkers>` (this component doesn't exist)
- ✅ Always specify `LatitudeValuePath` and `LongitudeValuePath` 
- ✅ Bind data with `DataSource="@YourDataList"`
- ✅ Data class **MUST** have `Latitude` and `Longitude` properties

## Marker Types and Styling

### Marker Shape Types

The `Shape` property determines the marker appearance. Use multiple `<MapsMarker>` components with different shapes:

```csharp
@using Syncfusion.Blazor.Maps

<SfMaps>
    <MapsLayers>
        <MapsLayer ShapeData='new { dataOptions = "https://cdn.syncfusion.com/maps/map-data/world-map.json" }' TValue="MarkerData">
            <MapsMarkerSettings>
                <!-- Circle marker -->
                <MapsMarker DataSource="@CircleMarkers" 
                            TValue="MarkerData"
                            LatitudeValuePath="Latitude"
                            LongitudeValuePath="Longitude"
                            Shape="MarkerType.Circle" Width="15" Height="15"></MapsMarker>
                
                <!-- Rectangle marker -->
                <MapsMarker DataSource="@RectMarkers" 
                            TValue="MarkerData"
                            LatitudeValuePath="Latitude"
                            LongitudeValuePath="Longitude"
                            Shape="MarkerType.Rectangle" Width="20" Height="20"></MapsMarker>
                
                <!-- Triangle marker -->
                <MapsMarker DataSource="@TriangleMarkers" 
                            TValue="MarkerData"
                            LatitudeValuePath="Latitude"
                            LongitudeValuePath="Longitude"
                            Shape="MarkerType.Triangle" Width="20" Height="20"></MapsMarker>
                
                <!-- Cross marker -->
                <MapsMarker DataSource="@CrossMarkers" 
                            TValue="MarkerData"
                            LatitudeValuePath="Latitude"
                            LongitudeValuePath="Longitude"
                            Shape="MarkerType.Cross" Width="20" Height="20"></MapsMarker>
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

    private List<MarkerData> CircleMarkers = new() { new MarkerData { Latitude = 37.368, Longitude = -122.095 } };
    private List<MarkerData> RectMarkers = new() { new MarkerData { Latitude = 40.7128, Longitude = -74.0060 } };
    private List<MarkerData> TriangleMarkers = new() { new MarkerData { Latitude = 34.0522, Longitude = -118.2437 } };
    private List<MarkerData> CrossMarkers = new() { new MarkerData { Latitude = 41.8781, Longitude = -87.6298 } };
}
```

**Supported Shape Types:**
- `Circle` - Default circular marker
- `Rectangle` - Square/rectangular marker
- `Triangle` - Triangular marker
- `Cross` - Cross/plus-sign marker
- `Diamond` - Diamond-shaped marker
- `Star` - Star-shaped marker
- `Balloon` - Balloon-style marker
- `Image` - Custom image marker

### Marker Styling

```csharp
@using Syncfusion.Blazor.Maps

<SfMaps>
    <MapsLayers>
        <MapsLayer ShapeData='new { dataOptions = "https://cdn.syncfusion.com/maps/map-data/world-map.json" }' TValue="MarkerData">
            <MapsMarkerSettings>
                <MapsMarker DataSource="@StyledMarkers" 
                            TValue="MarkerData"
                            LatitudeValuePath="Latitude"
                            LongitudeValuePath="Longitude"
                            Shape="MarkerType.Circle" 
                            Width="20" Height="20"
                            Fill="red"
                            Opacity="0.8">
                    <MapsMarkerBorder Color="darkred" Width="2"></MapsMarkerBorder>
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

    private List<MarkerData> StyledMarkers = new() { new MarkerData { Latitude = 37.368, Longitude = -122.095 } };
}
```

**Key Styling Properties:**
- `Fill`: Interior color (hex or named)
- `Opacity`: Transparency (0-1, default 1)
- `Width`: Marker width in pixels (default 10)
- `Height`: Marker height in pixels (default 10)
- `AnimationDuration`: Animation time in milliseconds
- `AnimationDelay`: Delay before animation starts

**MapsMarkerBorder class:**
- `Color`: Outline color
- `Width`: Outline width in pixels

### Image Markers

Use custom images for markers by setting `Shape="MarkerType.Image"`:

```csharp
@using Syncfusion.Blazor.Maps

<SfMaps>
    <MapsLayers>
        <MapsLayer ShapeData='new { dataOptions = "https://cdn.syncfusion.com/maps/map-data/world-map.json" }' TValue="string">
            <MapsMarkerSettings>
                <MapsMarker DataSource="@ImageMarkers" TValue="MarkerData"
                            Shape="MarkerType.Image"
                            ImageUrl="https://blazor.syncfusion.com/demos/_content/blazor_server_common_net8/images/maps/ballon.png"
                            Width="32" Height="32">
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

    private List<MarkerData> ImageMarkers = new() 
    { 
        new MarkerData { Latitude = 37.368, Longitude = -122.095 },
        new MarkerData { Latitude = 40.7128, Longitude = -74.0060 }
    };
}
```

**Image Recommendations:**
- Size: 32x32 to 64x64 pixels for clarity
- Format: PNG with transparency for best results
- Store in `/wwwroot/images/` directory
- Can use data URIs for inline SVG or base64 encoded images

## Marker Clustering

Automatically group nearby markers for cleaner visualization at low zoom levels. Enable clustering with `MapsMarkerClusterSettings`:

```csharp
@using Syncfusion.Blazor.Maps

<SfMaps>
    <MapsZoomSettings Enable="true"></MapsZoomSettings>
    <MapsLayers>
        <MapsLayer ShapeData='new { dataOptions = "https://cdn.syncfusion.com/maps/map-data/world-map.json" }' TValue="string">
            <MapsMarkerSettings>
                <MapsMarker DataSource="@LargeMarkerSet" TValue="CityData"
                            Shape="MarkerType.Circle" Width="15" Height="15">
                </MapsMarker>
            </MapsMarkerSettings>
            
            <!-- Enable Marker Clustering -->
            <MapsMarkerClusterSettings AllowClustering="true" 
                                       Shape="MarkerType.Circle" 
                                       Fill="#008CFF" 
                                       Height="25" Width="25">
                <MapsLayerMarkerClusterLabelStyle Color="white"></MapsLayerMarkerClusterLabelStyle>
            </MapsMarkerClusterSettings>
        </MapsLayer>
    </MapsLayers>
</SfMaps>

@code {
    public class CityData
    {
        public double Latitude { get; set; }
        public double Longitude { get; set; }
        public string Name { get; set; }
    }

    private List<CityData> LargeMarkerSet = new()
    {
        new CityData { Latitude = 40.6971494, Longitude = -74.2598747, Name = "New York" },
        new CityData { Latitude = 40.0024137, Longitude = -75.2581194, Name = "Philadelphia" },
        new CityData { Latitude = 42.3142647, Longitude = -71.11037, Name = "Boston" },
        new CityData { Latitude = 25.7823907, Longitude = -80.2994995, Name = "Miami" }
        // Add more cities as needed
    };
}
        </MapsLayer>
    </MapsLayers>
</SfMaps>

@code {
    private MapsCenterPosition CenterLatLng = new MapsCenterPosition { Latitude = 39.0, Longitude = -98.0 };
    
    private MapsMarkerClusterSettings ClusterSettings = new MapsMarkerClusterSettings
    {
        AllowClustering = true,
        Shape = MarkerType.Circle,
        Width = 40,
        Height = 40,
        BorderColor = "white",
        BorderWidth = 2,
        Fill = "orange"
    };

    private List<LocationData> Locations = new()
    {
        // Populate with many markers - will auto-cluster
        new LocationData { Latitude = 37.368, Longitude = -122.095 },
        new LocationData { Latitude = 37.380, Longitude = -122.100 },
        new LocationData { Latitude = 37.370, Longitude = -122.090 },
        // ... more locations ...
    };

    public class LocationData
    {
        public double Latitude { get; set; }
        public double Longitude { get; set; }
    }
}
```

**Cluster settings:**
- `AllowClustering`: Enable/disable clustering (bool, default: true)
- `AllowClusterExpand`: Allow cluster to expand on click (bool, default: true) ⭐ NEW
- `Shape`: Marker shape for clusters (MarkerType)
- `Width`/`Height`: Cluster circle size (double)
- `BorderColor`/`BorderWidth`: Outline styling
- `Fill`: Cluster circle color
- `ImageUrl`: Custom image for cluster shapes instead of geometric shapes ⭐ NEW
- `DashArray`: Dash pattern for cluster outline ⭐ NEW
- `ClusterDistance`: Distance in pixels for grouping nearby markers (int, default: 50)
- `LabelFill`: Text color for cluster count label

**Clustering benefits:**
- Reduces visual clutter at zoom level 1-6
- Auto-expands when user zooms in
- Improves performance with large marker sets (100+)

**Advanced Cluster Configuration Example:**
```csharp
<MapsMarkerClusterSettings AllowClustering="true" 
                            AllowClusterExpand="true"  // ← NEW
                            Shape="MarkerType.Circle" 
                            Width="40" Height="40"
                            Fill="orange"
                            BorderColor="white"
                            BorderWidth="2"
                            DashArray="5"              // ← NEW - Dashed outline
                            LabelFill="white"
                            ClusterDistance="50"
                            ImageUrl="">               // ← NEW - Leave empty for shape, or set to image URL
    <MapsLayerMarkerClusterLabelStyle Color="white"></MapsLayerMarkerClusterLabelStyle>
    <MapsLayerMarkerClusterConnectorLineSettings Color="blue" Opacity="0.5">
    </MapsLayerMarkerClusterConnectorLineSettings>
</MapsMarkerClusterSettings>
```

**Cluster Events (NEW):**
```csharp
<SfMaps OnMarkerClusterClick="OnClusterClick" OnMarkerClusterMove="OnClusterMove">
    <MapsEvents></MapsEvents>
    <!-- Map configuration -->
</SfMaps>

@code {
    private void OnClusterClick(MarkerClusterClickEventArgs args)
    {
        Console.WriteLine($"Cluster at ({args.Latitude}, {args.Longitude}) has {args.ClusterSize} markers");
    }

    private void OnClusterMove(MarkerClusterMoveEventArgs args)
    {
        Console.WriteLine($"Mouse hovering over cluster");
    }
}
```

## Understanding Layers

A layer is an ordered collection of visual elements rendered at a specific depth. Maps render layers sequentially, with later layers appearing on top.

**Layer types:**
- **Tile layer:** Base map (OpenStreetMap, Google Maps)
- **Shape layer:** Geographic shapes and polygons
- **Marker layer:** Point markers and annotations
- **Bubble layer:** Data bubbles and circles

## Working with Multiple Layers

### Stacking Layers

```csharp
<SfMaps >
<MapsZoomSettings ZoomFactor="4"></MapsZoomSettings>
    <MapsLayers>
        <!-- Base tile layer (renders first, appears underneath) -->
        <MapsLayer UrlTemplate="https://tile.openstreetmap.org/level/tileX/tileY.png" TValue="string">
        </MapsLayer>

        <!-- Shape layer (renders second, on top of tiles) -->
        <MapsLayer TValue="CountryData"
                   ShapeData='new { dataOptions = "https://cdn.syncfusion.com/maps/map-data/world-map.json" }'
                   DataSource="@CountryData"
                   ShapePropertyPath='new string[] { "name" }'>
            <MapsShapeSettings Fill="lightblue">
            </MapsShapeSettings>
        </MapsLayer>

        <!-- Marker layer (renders third, on top) -->
        <MapsLayer TValue="MarkerData">
            <MapsMarkerSettings>
                <MapsMarker TValue="MarkerData" Latitude="37.368" Longitude="-122.095" 
                    Shape="MarkerType.Circle" Width="15" Height="15" Visible="true">
                </MapsMarker>
            </MapsMarkerSettings>
        </MapsLayer>
    </MapsLayers>
</SfMaps>

@code {
    private MapsCenterPosition CenterLatLng = new MapsCenterPosition { Latitude = 39.0, Longitude = -98.0 };
    private List<object> CountryData = new();
}
```

### Independent Layer Styling

Each layer can have independent styling:

```csharp
<MapsLayers>
    <MapsLayer UrlTemplate="https://tile.openstreetmap.org/level/tileX/tileY.png" TValue="string">
        <MapsLayerOpacity>0.8</MapsLayerOpacity>
    </MapsLayer>

    <MapsLayer TValue="string">
        <MapsLayerOpacity>0.6</MapsLayerOpacity>
        <!-- Different opacity for this layer -->
    </MapsLayer>
</MapsLayers>
```

## Layer Visibility and Management

### Toggle Layer Visibility

Control which layers are visible:

```csharp
@page "/layer-toggle"
@using Syncfusion.Blazor.Maps

<button @onclick="() => ToggleLayer(0)">Toggle Base Map</button>
<button @onclick="() => ToggleLayer(1)">Toggle Markers</button>

<div style="width: 100%; height: 500px;">
    <SfMaps @ref="mapInstance" CenterPosition="@CenterLatLng" >
    <MapsZoomSettings ZoomFactor="4"></MapsZoomSettings>
        <MapsLayers>
            @for (int i = 0; i < Layers.Count; i++)
            {
                if (VisibleLayers.Contains(i))
                {
                    <MapsLayer @key="i" UrlTemplate="https://tile.openstreetmap.org/level/tileX/tileY.png" TValue="string">
                        @if (i == 1)
                        {
                            <MapsMarkerSettings>
                                <MapsMarker Latitude="37.368" Longitude="-122.095" 
                                    Shape="MarkerType.Circle" Width="15" Height="15">
                                </MapsMarker>
                            </MapsMarkerSettings>
                        }
                    </MapsLayer>
                }
            }
        </MapsLayers>
    </SfMaps>
</div>

@code {
    private SfMaps mapInstance;
    private MapsCenterPosition CenterLatLng = new MapsCenterPosition { Latitude = 37.368, Longitude = -122.095 };
    private List<string> Layers = new() { "Base Map", "Markers" };
    private List<int> VisibleLayers = new() { 0, 1 };

    private async Task ToggleLayer(int index)
    {
        if (VisibleLayers.Contains(index))
            VisibleLayers.Remove(index);
        else
            VisibleLayers.Add(index);

        await mapInstance.RefreshAsync();
    }
}
```

## Dynamic Marker Updates

### Adding Markers Programmatically

```csharp
@page "/dynamic-markers"
@using Syncfusion.Blazor.Maps

<button @onclick="AddMarker">Add Marker</button>
<button @onclick="ClearMarkers">Clear All</button>

<div style="width: 100%; height: 500px;">
    <SfMaps @ref="mapInstance" CenterPosition="@CenterLatLng" >
    <MapsZoomSettings ZoomFactor="4"></MapsZoomSettings>
        <MapsLayers>
            <MapsLayer UrlTemplate="https://tile.openstreetmap.org/level/tileX/tileY.png" TValue="string">
                <MapsMarkerSettings>
                    @foreach (var marker in DynamicMarkers)
                    {
                        <MapsMarker Latitude="@marker.Latitude" 
                            Longitude="@marker.Longitude"
                            Shape="MarkerType.Circle" 
                            Width="15" Height="15"
                            Fill="@marker.Color">
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
    private List<MarkerData> DynamicMarkers = new();
    private Random random = new();

    private async Task AddMarker()
    {
        var lat = 25 + (random.NextDouble() * 50); // Random US latitude
        var lon = -125 + (random.NextDouble() * 50); // Random US longitude
        var colors = new[] { "red", "blue", "green", "orange", "purple" };

        DynamicMarkers.Add(new MarkerData 
        { 
            Latitude = lat, 
            Longitude = lon, 
            Color = colors[random.Next(colors.Length)] 
        });

        await mapInstance.RefreshAsync();
    }

    private async Task ClearMarkers()
    {
        DynamicMarkers.Clear();
        await mapInstance.RefreshAsync();
    }

    public class MarkerData
    {
        public double Latitude { get; set; }
        public double Longitude { get; set; }
        public string Color { get; set; }
    }
}
```

### Updating Existing Markers

```csharp
private async Task UpdateMarkerColor(int index, string newColor)
{
    if (index >= 0 && index < DynamicMarkers.Count)
    {
        DynamicMarkers[index].Color = newColor;
        await mapInstance.RefreshAsync();
    }
}
```

### Real-Time Marker Updates

For continuous data (GPS tracking, live locations):

```csharp
@implements IAsyncDisposable

private CancellationTokenSource cts = new();

private async Task StartTracking()
{
    cts = new CancellationTokenSource();
    _ = UpdateMarkersInRealTime(cts.Token);
}

private async Task UpdateMarkersInRealTime(CancellationToken token)
{
    while (!token.IsCancellationRequested)
    {
        // Update marker positions
        for (int i = 0; i < DynamicMarkers.Count; i++)
        {
            DynamicMarkers[i].Latitude += (random.NextDouble() - 0.5) * 0.01;
            DynamicMarkers[i].Longitude += (random.NextDouble() - 0.5) * 0.01;
        }

        await mapInstance.RefreshAsync();
        await Task.Delay(1000, token); // Update every second
    }
}

private async Task StopTracking()
{
    cts.Cancel();
}

async ValueTask IAsyncDisposable.DisposeAsync()
{
    cts?.Cancel();
    cts?.Dispose();
}
```

## Performance Optimization

For large marker sets (100+):

1. **Use clustering:** Automatically groups nearby markers
2. **Use marker collections:** Bind to `List<T>` instead of individual markers
3. **Limit visible markers:** Only render markers in current viewport
4. **Use simpler shapes:** Circles render faster than images
5. **Update sparingly:** Batch updates rather than updating individually

## MapsMarker<TValue> API Reference

Generic marker component with data binding support.

### Key Properties

```csharp
<MapsMarker Latitude="37.368" 
            Longitude="-122.095"
            Shape="MarkerType.Circle"
            Width="15"
            Height="15"
            Fill="red"
            Opacity="0.8"
            ImageUrl=""
            ColorValuePath="color"
            HeightValuePath="height"
            WidthValuePath="width"
            ShapeValuePath="shape"
            Visible="true"
            EnableDrag="false">
</MapsMarker>
```

| Property | Type | Description |
|----------|------|-------------|
| `Latitude` | double | Y-coordinate (-90 to 90) - Set via component attribute or data binding |
| `Longitude` | double | X-coordinate (-180 to 180) - Set via component attribute or data binding |
| `Shape` | `MarkerType` | Marker shape: Circle, Rectangle, Triangle, Cross, Diamond, Image, Balloon. Default: Balloon |
| `Width` | double | Marker width in pixels. Default: 10 |
| `Height` | double | Marker height in pixels. Default: 10 |
| `Fill` | string | Marker color (hex or named). Default: "#FF471A" |
| `Opacity` | double | Transparency (0-1). Default: 1 |
| `ImageUrl` | string | Custom image URL (when Shape is Image type) |
| `ColorValuePath` | string | Data field for dynamic marker color |
| `HeightValuePath` | string | Data field for dynamic marker height |
| `WidthValuePath` | string | Data field for dynamic marker width |
| `ShapeValuePath` | string | Data field for dynamic marker shape |
| `ImageUrlValuePath` | string | Data field for dynamic image URL |
| `LatitudeValuePath` | string | Data field for latitude values |
| `LongitudeValuePath` | string | Data field for longitude values |
| `Visible` | bool | Show/hide marker. Default: true |
| `EnableDrag` | bool | Allow marker dragging. Default: false |
| `AnimationDuration` | double | Animation duration in milliseconds. Default: 1000 |
| `AnimationDelay` | double | Animation delay in milliseconds. Default: 0 |
| `OffsetX` | double | Horizontal offset from longitude |
| `OffsetY` | double | Vertical offset from latitude |
| `DashArray` | string | Dash pattern for marker outline |
| `LegendText` | string | Legend label for marker |

## MapsLayer<TValue> API Reference

Generic layer component for rendering geographic data.

### Key Properties

```csharp
<MapsLayer UrlTemplate="https://tile.openstreetmap.org/level/tileX/tileY.png" TValue="string"
           ShapeDataSource="@shapeData"
           DataSource="@dataSource"
           ShapePropertyPath="name"
           ShapeSettings="@shapeSettings"
           MarkerSettings="@markerSettings">
</MapsLayer>
```

| Property | Type | Description |
|----------|------|-------------|
| `UrlTemplate` | string | Tile provider URL template |
| `Type` | `Type` | Layer type: GeometryNormalShape, ShapeGeometry, OSMShapeGeometry |
| `ShapeDataSource` | object | GeoJSON data for shapes |
| `DataSource` | object | Data for color/value mapping |
| `ShapePropertyPath` | string | Data field for shape matching |
| `ShapeSettings` | `MapsShapeSettings` | Shape appearance configuration |
| `MarkerSettings` | `MapsMarkerSettings` | Marker appearance defaults |

## MapsMarkerClusterSettings API

Configure marker clustering behavior.

```csharp
<MapsMarkerClusterSettings AllowClustering="true"
                            Shape="MarkerType.Circle"
                            Width="40"
                            Height="40"
                            Fill="orange"
                            BorderColor="white"
                            BorderWidth="2"
                            LabelFill="white"
                            ClusterDistance="50">
</MapsMarkerClusterSettings>
```

| Property | Type | Description |
|----------|------|-------------|
| `AllowClustering` | bool | Enable/disable clustering |
| `Shape` | `MarkerType` | Cluster shape |
| `Width` | double | Cluster width |
| `Height` | double | Cluster height |
| `Fill` | string | Cluster color |
| `BorderColor` | string | Cluster border color |
| `BorderWidth` | double | Cluster border width |
| `LabelFill` | string | Text color in cluster |
| `ClusterDistance` | int | Distance in pixels for grouping |

## MapsMarkerSettings API

Set default marker appearance for all markers.

```csharp
<MapsMarkerSettings Fill="blue"
                    Opacity="0.8"
                    Width="15"
                    Height="15"
                    Border="@markerBorder"
                    Shape="MarkerType.Circle">
</MapsMarkerSettings>
```

## MapsShapeSettings API

Configure shape/polygon appearance.

```csharp
<MapsShapeSettings Fill="lightblue"
                   Stroke="blue"
                   StrokeWidth="2"
                   Opacity="0.7"
                   ColorValuePath="population">
</MapsShapeSettings>
```

| Property | Type | Description |
|----------|------|-------------|
| `Fill` | string | Shape fill color |
| `Stroke` | string | Border color |
| `StrokeWidth` | double | Border width |
| `Opacity` | double | Transparency (0-1) |
| `ColorValuePath` | string | Data field for color mapping |


