# Events and Methods

## Table of Contents

- [Mouse Events](#mouse-events)
   - [OnMouseMove](#onmousemove)
   - [OnMarkerClick](#onmarkerclick)
   - [OnShapeSelected](#onshapeselected)
   - [OnDoubleClick](#ondoubleclick)
   - [🔧 CRITICAL FIX: OnMouseMove - Wrong Event Argument Class](#-critical-fix-onmousemove---wrong-event-argument-class)
   - [OnMarkerMouseLeave](#onmarkermouseleave)
   - [OnMarkerClusterClick](#onmarkerclusterclick)
   - [OnMarkerClusterMove](#onmarkerclustermove)
- [Pan and Zoom Events](#pan-and-zoom-events)
   - [OnZoomChange](#onzoomchange)
   - [OnPan](#onpan)
- [Rendering Events (Pre-Render Hooks)](#rendering-events-pre-render-hooks)
   - [OnShapeRendering](#onshaperendering)
   - [OnLayerRendering](#onlayerrendering)
   - [OnAnnotationRendering](#onannotationrendering)
- [Map Methods](#map-methods)
   - [Refresh()](#refresh)
   - [ZoomToCoordinates()](#zoomtocoordinates)
   - [ZoomByPosition()](#zoombyposition)
   - [GetMinMaxLatitudeLongitude()](#getminmaxlatitudelongitude)
   - [ShapeSelectionAsync()](#shapeselectionasync)
- [Event Data Handling](#event-data-handling)
   - [Capturing Event Arguments](#capturing-event-arguments)
   - [Event Filtering](#event-filtering)
- [Common Event Patterns](#common-event-patterns)
   - [Update Display on Selection](#update-display-on-selection)
   - [Async Event Handling](#async-event-handling)
   - [Event Debouncing](#event-debouncing)
- [SfMaps Methods API Reference](#sfmaps-methods-api-reference)
   - [Pan Methods](#pan-methods)
   - [Refresh](#refresh)
   - [Export and Print](#export-and-print)
   - [Bounds and Coordinates](#bounds-and-coordinates)
- [Events API Reference](#events-api-reference)
   - [Complete Event Binding Example](#complete-event-binding-example)
   - [Available Events](#available-events)
   - [Event Arguments Details](#event-arguments-details)


## Mouse Events

### OnMouseMove

Triggered as user moves mouse over the map:

```csharp
@page "/mouse-move"
@using Syncfusion.Blazor.Maps

<p>Current position: @MousePos</p>

<SfMaps @ref="mapInstance" OnMouseMove="MouseMoveHandler">
    <MapsLayers>
        <MapsLayer UrlTemplate="https://tile.openstreetmap.org/level/tileX/tileY.png" TValue="string">
        </MapsLayer>
    </MapsLayers>
</SfMaps>

@code {
    private SfMaps mapInstance;
    private string MousePos = "Move mouse over map";

    private void MouseMoveHandler(MapClickEventArgs args)
    {
        MousePos = $"Lat: {args.Latitude:F2}, Lng: {args.Longitude:F2}";
    }
}
```

### OnMarkerClick

Triggered when user clicks a marker:

```csharp
<SfMaps @ref="mapInstance" OnMarkerClick="MarkerClickHandler">
    <MapsLayers>
        <MapsLayer UrlTemplate="https://tile.openstreetmap.org/level/tileX/tileY.png" TValue="string">
            <MapsMarkerSettings>
                <MapsMarker Latitude="37.368" Longitude="-122.095" 
                    Shape="MarkerType.Circle" Width="15" Height="15">
                </MapsMarker>
            </MapsMarkerSettings>
        </MapsLayer>
    </MapsLayers>
</SfMaps>

@code {
    private void MarkerClickHandler(MarkerClickEventArgs args)
    {
        Console.WriteLine($"Marker clicked at Lat: {args.Latitude}, Lng: {args.Longitude}");
        // Update UI, show details, trigger navigation, etc.
    }
}
```

**Event args:**
- `Latitude`: Marker latitude coordinate
- `Longitude`: Marker longitude coordinate
- `MarkerType`: Type of marker (Circle, Rectangle, etc.)
- `Color`: Marker fill color

### OnShapeSelected

Triggered when user selects (clicks) a shape/polygon:

```csharp
<SfMaps @ref="mapInstance" OnShapeSelected="ShapeSelectHandler">
    <MapsLayers>
        <MapsLayer TValue="StateData" DataSource="@StateData">
            <MapsShapeSettings Fill="lightblue">
            </MapsShapeSettings>
        </MapsLayer>
    </MapsLayers>
</SfMaps>

@code {
    private void ShapeSelectHandler(ShapeSelectedEventArgs args)
    {
        Console.WriteLine($"Selected shape data: {args.ShapeData}");
        // Access selected region properties
    }
}
```

**Event args:**
- `ShapeData`: Data properties of the selected shape
- `GeometryType`: Type of geometry (Polygon, LineString, Point)

### OnDoubleClick

Triggered on double-click anywhere on map:

```csharp
<SfMaps OnDoubleClick="DoubleClickHandler">
    <MapsLayers>
        <MapsLayer UrlTemplate="https://tile.openstreetmap.org/level/tileX/tileY.png" TValue="string">
        </MapsLayer>
    </MapsLayers>
</SfMaps>

@code {
    private void DoubleClickHandler(MapClickEventArgs args)
    {
        Console.WriteLine($"Double-clicked at: {args.Latitude}, {args.Longitude}");
        // Could zoom in, add marker, etc.
    }
}
```

### 🔧 CRITICAL FIX: OnMouseMove - Wrong Event Argument Class

**❌ INCORRECT (Currently Documented):**
```csharp
private void MouseMoveHandler(MapClickEventArgs args) // WRONG CLASS!
{
    // MapClickEventArgs is for click events, not mouse move
}
```

**✅ CORRECT:**
```csharp
@page "/mouse-move"
@using Syncfusion.Blazor.Maps

<SfMaps OnMouseMove="MouseMoveHandler">
    <MapsLayers>
        <MapsLayer UrlTemplate="https://tile.openstreetmap.org/level/tileX/tileY.png" TValue="string">
        </MapsLayer>
    </MapsLayers>
</SfMaps>

@code {
    // Correct event argument class: MouseMoveEventArgs
    private void MouseMoveHandler(MouseMoveEventArgs args)
    {
        Console.WriteLine($"Mouse at page position: X={args.Page?.X}, Y={args.Page?.Y}");
        // Note: MouseMoveEventArgs provides page coordinates, not map coordinates
    }
}
```

**Key Difference:** 
- `MapClickEventArgs` → For click events (provides Latitude/Longitude)
- `MouseMoveEventArgs` → For mouse move events (provides page X/Y coordinates)

---

### OnMarkerMouseLeave

Triggered when mouse leaves a marker (NEW - Previously Missing):

```csharp
<SfMaps OnMarkerMouseLeave="MarkerMouseLeaveHandler">
    <MapsLayers>
        <MapsLayer UrlTemplate="https://tile.openstreetmap.org/level/tileX/tileY.png" TValue="string">
            <MapsMarkerSettings>
                <MapsMarker DataSource="@Markers" TValue="MarkerData"
                            Shape="MarkerType.Circle" Width="15" Height="15">
                </MapsMarker>
            </MapsMarkerSettings>
        </MapsLayer>
    </MapsLayers>
</SfMaps>

@code {
    private void MarkerMouseLeaveHandler(MarkerMouseLeaveEventArgs args)
    {
        Console.WriteLine($"Mouse left marker at ({args.Latitude}, {args.Longitude})");
        // Cleanup tooltip, reset styles, etc.
    }

    public class MarkerData
    {
        public double Latitude { get; set; }
        public double Longitude { get; set; }
    }

    private List<MarkerData> Markers = new()
    {
        new MarkerData { Latitude = 37.368, Longitude = -122.095 }
    };
}
```

---

### OnMarkerClusterClick

Triggered when user clicks a cluster of markers (NEW - Previously Missing):

```csharp
<SfMaps OnMarkerClusterClick="ClusterClickHandler">
    <MapsLayers>
        <MapsLayer UrlTemplate="https://tile.openstreetmap.org/level/tileX/tileY.png" TValue="string">
            <MapsMarkerSettings>
                <MapsMarker DataSource="@LargeMarkerSet" TValue="CityData"
                            Shape="MarkerType.Circle" Width="15" Height="15">
                </MapsMarker>
            </MapsMarkerSettings>
            <MapsMarkerClusterSettings AllowClustering="true" 
                                       Shape="MarkerType.Circle" 
                                       Fill="#008CFF" Height="25" Width="25">
            </MapsMarkerClusterSettings>
        </MapsLayer>
    </MapsLayers>
</SfMaps>

@code {
    private void ClusterClickHandler(MarkerClusterClickEventArgs args)
    {
        Console.WriteLine($"Cluster clicked at ({args.Latitude}, {args.Longitude})");
        Console.WriteLine($"Cluster contains {args.ClusterSize} markers");
        Console.WriteLine($"Data: {string.Join(", ", args.Data)}");
        // Handle cluster interaction - expand, show details, etc.
    }

    public class CityData
    {
        public double Latitude { get; set; }
        public double Longitude { get; set; }
        public string Name { get; set; }
    }

    private List<CityData> LargeMarkerSet = new() { /* populated with many cities */ };
}
```

**Event Args Properties:**
- `Latitude`: Cluster center latitude
- `Longitude`: Cluster center longitude
- `ClusterSize`: Number of markers in the cluster
- `Data`: Array of clustered marker data

---

### OnMarkerClusterMove

Triggered when mouse moves over a cluster (NEW - Previously Missing):

```csharp
<SfMaps OnMarkerClusterMove="ClusterMoveHandler">
    <MapsLayers>
        <MapsLayer UrlTemplate="https://tile.openstreetmap.org/level/tileX/tileY.png" TValue="string">
            <MapsMarkerSettings>
                <MapsMarker DataSource="@LargeMarkerSet" TValue="CityData"
                            Shape="MarkerType.Circle" Width="15" Height="15">
                </MapsMarker>
            </MapsMarkerSettings>
            <MapsMarkerClusterSettings AllowClustering="true" 
                                       Shape="MarkerType.Circle" 
                                       Fill="#008CFF" Height="25" Width="25">
            </MapsMarkerClusterSettings>
        </MapsLayer>
    </MapsLayers>
</SfMaps>

@code {
    private void ClusterMoveHandler(MarkerClusterMoveEventArgs args)
    {
        Console.WriteLine($"Mouse over cluster at ({args.Latitude}, {args.Longitude})");
        // Show tooltip, highlight cluster, etc.
    }

    public class CityData
    {
        public double Latitude { get; set; }
        public double Longitude { get; set; }
    }

    private List<CityData> LargeMarkerSet = new() { /* populated */ };
}
```

## Pan and Zoom Events

### OnZoomChange

Triggered when zoom level changes (user zoom, programmatic zoom):

```csharp
@page "/zoom-event"
@using Syncfusion.Blazor.Maps

<p>Current zoom level: @CurrentZoomLevel</p>

<SfMaps @ref="mapInstance" OnZoomChange="ZoomChangeHandler" >
<MapsZoomSettings ZoomFactor="4"></MapsZoomSettings>
    <MapsLayers>
        <MapsLayer UrlTemplate="https://tile.openstreetmap.org/level/tileX/tileY.png" TValue="string">
        </MapsLayer>
    </MapsLayers>
</SfMaps>

@code {
    private SfMaps mapInstance;
    private int CurrentZoomLevel = 4;

    private void ZoomChangeHandler(ZoomEventArgs args)
    {
        CurrentZoomLevel = (int)args.NewZoomLevel;
        Console.WriteLine($"Zoomed to level: {args.NewZoomLevel}");
        // Load new data, adjust display, etc.
    }
}
```

**Event args:**
- `NewZoomLevel`: The new zoom level
- `PreviousZoomLevel`: The previous zoom level
- `Type`: "ZoomIn", "ZoomOut", or programmatic

### OnPan

Triggered when map is panned:

```csharp
<SfMaps OnPan="PanHandler">
    <MapsLayers>
        <MapsLayer UrlTemplate="https://tile.openstreetmap.org/level/tileX/tileY.png" TValue="string">
        </MapsLayer>
    </MapsLayers>
</SfMaps>

@code {
    private void PanHandler(MapPanEventArgs args)
    {
        Console.WriteLine($"Panned to: Lat {args.NewCenterPosition.Latitude}, Lng {args.NewCenterPosition.Longitude}");
        // Update coordinates display, load region-specific data, etc.
    }
}
```

## Rendering Events (Pre-Render Hooks)

Pre-render events allow you to customize appearance before rendering. Use these to modify colors, styles, or content based on data conditions.

### OnShapeRendering

Triggered before each shape/polygon is rendered (NEW - Previously Underdocumented):

```csharp
@page "/shape-rendering"
@using Syncfusion.Blazor.Maps

<SfMaps>
    <MapsEvents OnShapeRendering="OnShapeRendering">
    </MapsEvents>
    <MapsLayers>
        <MapsLayer TValue="StateData" 
                   ShapeDataSource="@ShapeData"
                   DataSource="@StateData">
            <MapsShapeSettings Fill="lightblue">
            </MapsShapeSettings>
        </MapsLayer>
    </MapsLayers>
</SfMaps>

@code {
    private void OnShapeRendering(ShapeRenderingEventArgs args)
    {
        // Customize shape based on data
        if (args.ShapeData != null)
        {
            var shapeName = args.ShapeData.Properties?.Name;
            
            // Example: Highlight specific states
            if (shapeName == "California")
                args.Fill = "red";
            else if (shapeName == "Texas")
                args.Fill = "blue";
            else
                args.Fill = "lightblue";
        }
        
        Console.WriteLine($"Rendering shape: {args.ShapeData?.Properties?.Name}");
    }

    private object ShapeData = /* GeoJSON data */;
    private List<StateData> StateData = /* data source */;

    public class StateData
    {
        public string Name { get; set; }
        public int Population { get; set; }
    }
}
```

**Event Args:**
- `ShapeData`: The shape's data object
- `Fill`: Customize shape fill color (get/set)
- `Opacity`: Customize transparency (get/set)

---

### OnLayerRendering

Triggered before each layer is rendered (NEW - Previously Underdocumented):

```csharp
<SfMaps>
    <MapsEvents OnLayerRendering="OnLayerRendering">
    </MapsEvents>
    <MapsLayers>
        <MapsLayer UrlTemplate="https://tile.openstreetmap.org/level/tileX/tileY.png" TValue="string">
        </MapsLayer>
        <MapsLayer TValue="string" ShapeDataSource="@ShapeData">
        </MapsLayer>
    </MapsLayers>
</SfMaps>

@code {
    private void OnLayerRendering(LayerRenderingEventArgs args)
    {
        Console.WriteLine($"Rendering layer: {args.LayerType}");
        // Apply layer-level customizations, load data, etc.
    }

    private object ShapeData = /* GeoJSON data */;
}
```

---

### OnAnnotationRendering

Triggered before each annotation is rendered (NEW - Previously Underdocumented):

```csharp
<SfMaps>
    <MapsEvents OnAnnotationRendering="OnAnnotationRendering">
    </MapsEvents>
    <MapsLayers>
        <MapsLayer UrlTemplate="https://tile.openstreetmap.org/level/tileX/tileY.png" TValue="string">
            <MapsAnnotations>
                <MapsAnnotation Latitude="37.368" Longitude="-122.095"
                    Content="Default Label">
                </MapsAnnotation>
            </MapsAnnotations>
        </MapsLayer>
    </MapsLayers>
</SfMaps>

@code {
    private void OnAnnotationRendering(AnnotationRenderingEventArgs args)
    {
        // Customize annotation content before rendering
        args.Content = $"<div style='background: yellow; padding: 5px;'>{args.Content}</div>";
        Console.WriteLine($"Rendering annotation: {args.Content}");
    }
}
```

**Event Args:**
- `Content`: The annotation HTML content (get/set)
- `AnnotationData`: Associated data object

---

## Map Methods

### Refresh()

Redraw the map (updates layers, markers, data bindings):

```csharp
<button @onclick="RefreshMapData">Refresh</button>

@code {
    private SfMaps mapInstance;

    private void RefreshMapData()
    {
        // Update data
        MapData = FetchNewData();
        // Refresh display
        mapInstance.Refresh();
    }
}
```

**Note:** This is a synchronous method, not async.

---

### ZoomToCoordinates()

Zoom the map to fit specific geographic coordinates:

```csharp
<button @onclick="() => ZoomToRegion()">Zoom to California</button>

@code {
    private SfMaps mapInstance;

    private void ZoomToRegion()
    {
        // Zoom to fit coordinates
        // Parameters: minLatitude, minLongitude, maxLatitude, maxLongitude
        mapInstance.ZoomToCoordinates(
            minLatitude: 32.5,
            minLongitude: -124.4,
            maxLatitude: 42.0,
            maxLongitude: -114.6
        );
    }
}
```

---

### ZoomByPosition()

Zoom the map from a specific center position with a zoom factor:

```csharp
<button @onclick="() => ZoomToLocation(37.368, -122.095, 2.0)">Zoom to SF</button>

@code {
    private SfMaps mapInstance;

    private void ZoomToLocation(double lat, double lng, double zoomFactor)
    {
        var center = new MapsCenterPosition { Latitude = lat, Longitude = lng };
        mapInstance.ZoomByPosition(center, zoomFactor);
    }
}
```

---

### GetMinMaxLatitudeLongitude()

Get the minimum and maximum coordinates of the visible map area:

```csharp
<button @onclick="GetMapBounds">Get Visible Bounds</button>

@code {
    private SfMaps mapInstance;

    private void GetMapBounds()
    {
        var bounds = mapInstance.GetMinMaxLatitudeLongitude();
        Console.WriteLine($"Min Lat: {bounds.MinLatitude}, Max Lat: {bounds.MaxLatitude}");
        Console.WriteLine($"Min Lng: {bounds.MinLongitude}, Max Lng: {bounds.MaxLongitude}");
    }
}
```

**Note:** This is a synchronous method.

---

### ShapeSelectionAsync()

Programmatically select or unselect shapes in the map:

```csharp
<button @onclick="() => SelectShape()">Select California</button>
<button @onclick="() => DeselectShape()">Deselect California</button>

@code {
    private SfMaps mapInstance;

    private async Task SelectShape()
    {
        // Parameters: layerIndex, propertyName, name, enable
        await mapInstance.ShapeSelectionAsync(
            layerIndex: 0,
            propertyName: "name",
            name: "California",
            enable: true  // Select
        );
    }

    private async Task DeselectShape()
    {
        await mapInstance.ShapeSelectionAsync(
            layerIndex: 0,
            propertyName: "name",
            name: "California",
            enable: false  // Deselect
        );
    }
}
```

## Event Data Handling

### Capturing Event Arguments

Access full event data in handlers:

```csharp
@page "/event-data"
@using Syncfusion.Blazor.Maps

<div>
    <p>Last event type: @LastEventType</p>
    <p>Event details: @EventDetails</p>
</div>

<SfMaps @ref="mapInstance" 
    OnMouseMove="MouseMoveHandler"
    OnMarkerClick="MarkerClickHandler"
    OnShapeSelected="ShapeSelectHandler">
    <MapsLayers>
        <MapsLayer UrlTemplate="https://tile.openstreetmap.org/level/tileX/tileY.png" TValue="string">
            <MapsMarkerSettings>
                <MapsMarker Latitude="37.368" Longitude="-122.095">
                </MapsMarker>
            </MapsMarkerSettings>
        </MapsLayer>
    </MapsLayers>
</SfMaps>

@code {
    private SfMaps mapInstance;
    private string LastEventType = "None";
    private string EventDetails = "";

    private void MouseMoveHandler(MapClickEventArgs args)
    {
        LastEventType = "MouseMove";
        EventDetails = $"Position: ({args.Latitude:F2}, {args.Longitude:F2})";
    }

    private void MarkerClickHandler(MarkerClickEventArgs args)
    {
        LastEventType = "MarkerClick";
        EventDetails = $"Marker at ({args.Latitude}, {args.Longitude})";
    }

    private void ShapeSelectHandler(ShapeSelectedEventArgs args)
    {
        LastEventType = "ShapeSelected";
        EventDetails = $"Shape data: {args.ShapeData}";
    }
}
```

### Event Filtering

Only process specific events:

```csharp
@page "/filtered-events"
@using Syncfusion.Blazor.Maps

<SfMaps @ref="mapInstance" OnMarkerClick="MarkerClickHandler">
    <MapsLayers>
        <MapsLayer UrlTemplate="https://tile.openstreetmap.org/level/tileX/tileY.png" TValue="string">
            <MapsMarkerSettings>
                <MapsMarker Latitude="37.368" Longitude="-122.095" 
                    Shape="MarkerType.Circle" Width="15" Height="15">
                </MapsMarker>
            </MapsMarkerSettings>
        </MapsLayer>
    </MapsLayers>
</SfMaps>

@code {
    private SfMaps mapInstance;

    private void MarkerClickHandler(MarkerClickEventArgs args)
    {
        // Only process markers within certain bounds
        if (args.Latitude >= 30 && args.Latitude <= 40 && 
            args.Longitude >= -125 && args.Longitude <= -115)
        {
            Console.WriteLine("Marker in valid region, processing...");
            // Process marker
        }
        else
        {
            Console.WriteLine("Marker outside valid region, ignoring");
        }
    }
}
```

## Common Event Patterns

### Update Display on Selection

```csharp
@page "/selection-display"
@using Syncfusion.Blazor.Maps

<div>
    @if (SelectedMarker != null)
    {
        <div style="background: lightblue; padding: 10px; margin: 10px;">
            <h3>Selected Location</h3>
            <p>Latitude: @SelectedMarker.Latitude</p>
            <p>Longitude: @SelectedMarker.Longitude</p>
            <button @onclick="ClearSelection">Deselect</button>
        </div>
    }
</div>

<SfMaps @ref="mapInstance" OnMarkerClick="MarkerClickHandler">
    <MapsLayers>
        <MapsLayer UrlTemplate="https://tile.openstreetmap.org/level/tileX/tileY.png" TValue="string">
            <MapsMarkerSettings>
                <MapsMarker Latitude="37.368" Longitude="-122.095"
                    Fill="@(SelectedMarker?.Latitude == 37.368 ? "red" : "blue")"
                    Shape="MarkerType.Circle" Width="15" Height="15">
                </MapsMarker>
            </MapsMarkerSettings>
        </MapsLayer>
    </MapsLayers>
</SfMaps>

@code {
    private SfMaps mapInstance;

    public class SelectedLocation
    {
        public double Latitude { get; set; }
        public double Longitude { get; set; }
    }

    private SelectedLocation SelectedMarker = null;

    private void MarkerClickHandler(MarkerClickEventArgs args)
    {
        SelectedMarker = new SelectedLocation 
        { 
            Latitude = args.Latitude, 
            Longitude = args.Longitude 
        };
    }

    private void ClearSelection()
    {
        SelectedMarker = null;
    }
}
```

### Async Event Handling

Perform async operations in event handlers:

```csharp
@page "/async-events"
@using Syncfusion.Blazor.Maps

<SfMaps @ref="mapInstance" OnMarkerClick="MarkerClickAsync">
    <MapsLayers>
        <MapsLayer UrlTemplate="https://tile.openstreetmap.org/level/tileX/tileY.png" TValue="string">
            <MapsMarkerSettings>
                <MapsMarker Latitude="37.368" Longitude="-122.095">
                </MapsMarker>
            </MapsMarkerSettings>
        </MapsLayer>
    </MapsLayers>
</SfMaps>

@if (IsLoading)
{
    <p>Loading details...</p>
}
else if (LocationDetails != null)
{
    <p>@LocationDetails</p>
}

@code {
    private SfMaps mapInstance;
    private bool IsLoading = false;
    private string LocationDetails = null;

    private async Task MarkerClickAsync(MarkerClickEventArgs args)
    {
        IsLoading = true;

        // Fetch location details from API
        var details = await FetchLocationDetails(args.Latitude, args.Longitude);
        LocationDetails = details;

        IsLoading = false;
    }

    private async Task<string> FetchLocationDetails(double lat, double lng)
    {
        // Simulate API call
        await Task.Delay(500);
        return $"Location: {lat}, {lng}";
    }
}
```

### Event Debouncing

Limit event processing frequency:

```csharp
@page "/debounced-events"
@using Syncfusion.Blazor.Maps
@implements IAsyncDisposable

<SfMaps @ref="mapInstance" OnMouseMove="MouseMoveHandler">
    <MapsLayers>
        <MapsLayer UrlTemplate="https://tile.openstreetmap.org/level/tileX/tileY.png" TValue="string">
        </MapsLayer>
    </MapsLayers>
</SfMaps>

<p>Updates per second: @UpdatesPerSecond</p>

@code {
    private SfMaps mapInstance;
    private DateTime LastUpdate = DateTime.Now;
    private int UpdateCount = 0;
    private int UpdatesPerSecond = 0;
    private Timer updateTimer;

    protected override void OnInitialized()
    {
        updateTimer = new Timer(_ =>
        {
            UpdatesPerSecond = UpdateCount;
            UpdateCount = 0;
        }, null, TimeSpan.FromSeconds(1), TimeSpan.FromSeconds(1));
    }

    private void MouseMoveHandler(MapClickEventArgs args)
    {
        var now = DateTime.Now;
        var timeSinceLastUpdate = (now - LastUpdate).TotalMilliseconds;

        // Only process if 100ms has passed since last update
        if (timeSinceLastUpdate >= 100)
        {
            LastUpdate = now;
            UpdateCount++;
            // Process event
        }
    }

    async ValueTask IAsyncDisposable.DisposeAsync()
    {
        updateTimer?.Dispose();
    }
}
```

## SfMaps Methods API Reference

### Pan Methods

```csharp
// Pan in specific direction
// Direction values: Bottom, Left, Right, Top
var position = new Syncfusion.Blazor.Maps.Internal.Point { X = 120, Y = 200 };
mapInstance.PanByDirectionAsync(PanDirection.Bottom, position);
```

### Refresh

```csharp
// Refresh map rendering (updates all layers, markers, data bindings)
await mapInstance.RefreshAsync();

```

### Export and Print

```csharp
// Export as PNG
await mapInstance.ExportAsync(ExportType.PNG, "map-export.png");

// Export as SVG
await mapInstance.ExportAsync(ExportType.SVG, "map-export.svg");

// Export as PDF
await mapInstance.ExportAsync(ExportType.PDF, "map-export.pdf");

// Print map
await mapInstance.PrintAsync();
```

### Bounds and Coordinates

```csharp
// Get current map bounds
var bounds = mapInstance.GetMinMaxLatitudeLongitude();
// Returns: MinLatitude, MaxLatitude, MinLongitude, MaxLongitude
```

## Events API Reference

### Complete Event Binding Example

```csharp
<MapsEvents OnMarkerClick="MarkerClickHandler"
            OnMarkerDragStart="DragStartHandler"
            OnMarkerDragEnd="DragEndHandler"
            OnShapeSelected="ShapeSelectHandler"
            OnZoomChange="ZoomChangeHandler"
            OnPan="PanHandler"
            OnLoad="LoadHandler"
            OnLoaded="LoadedHandler"
            OnMouseMove="MouseMoveHandler"
            OnDoubleClick="DoubleClickHandler"
            OnPrint="PrintHandler"
            OnResize="ResizeHandler">
</MapsEvents>
```

### Available Events

| Event | Arguments | Trigger |
|-------|-----------|---------|
| `OnMarkerClick` | `MarkerClickEventArgs` | Marker clicked |
| `OnMarkerDragStart` | `MarkerDragStartEventArgs` | Marker drag begins |
| `OnMarkerDragEnd` | `MarkerDragEndEventArgs` | Marker drag ends |
| `OnMarkerMove` | `MarkerMoveEventArgs` | Marker moving (dragging) |
| `OnMarkerMouseLeave` | `MarkerMouseLeaveEventArgs` | Mouse leaves marker |
| `OnBubbleClick` | `BubbleClickEventArgs` | Bubble clicked |
| `OnBubbleMove` | `BubbleMoveEventArgs` | Mouse moves over bubble |
| `OnShapeSelected` | `ShapeSelectedEventArgs` | Shape/polygon selected |
| `OnZoomChange` | `MapZoomEventArgs` | Zoom level changed |
| `OnPan` | `MapPanEventArgs` | Map panned |
| `OnMouseMove` | `MouseMoveEventArgs` | Mouse moves on map |
| `OnDoubleClick` | `MapClickEventArgs` | Double-click on map |
| `OnLoad` | `LoadEventArgs` | Before map loads |
| `OnLoaded` | `LoadedEventArgs` | After map loads |
| `OnShapeRendering` | `ShapeRenderingEventArgs` | Before shape renders |
| `OnMarkerRendering` | `MarkerRenderingEventArgs` | Before marker renders |
| `OnBubbleRendering` | `BubbleRenderingEventArgs` | Before bubble renders |
| `OnLabelRendering` | `LabelRenderingEventArgs` | Before label renders |
| `OnLayerRendering` | `LayerRenderingEventArgs` | Before layer renders |
| `OnLegendRendering` | `LegendRenderingEventArgs` | Before legend renders |
| `OnPrint` | `PrintEventArgs` | Before printing |
| `OnResize` | `ResizeEventArgs` | Map resized |
| `OnAnimationComplete` | `AnimationCompleteEventArgs` | Animation finished |

### Event Arguments Details

```csharp
// MarkerClickEventArgs
public class MarkerClickEventArgs
{
    public double Latitude { get; set; }
    public double Longitude { get; set; }
    public MarkerType MarkerType { get; set; }
    public string Color { get; set; }
    public object Data { get; set; }
}

// MapZoomEventArgs
public class MapZoomEventArgs
{
    public double PreviousZoomLevel { get; set; }
    public double NewZoomLevel { get; set; }
    public string Type { get; set; }  // "ZoomIn", "ZoomOut"
}

// MapPanEventArgs
public class MapPanEventArgs
{
    public MapsCenterPosition CurrentCenterPosition { get; set; }
    public MapsCenterPosition NewCenterPosition { get; set; }
}

// ShapeSelectedEventArgs
public class ShapeSelectedEventArgs
{
    public object ShapeData { get; set; }
    public GeometryType GeometryType { get; set; }
}
```

For complete event arguments, see [references/api-reference.md](api-reference.md#event-arguments)


