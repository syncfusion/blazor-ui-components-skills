# Events and Methods

> **MANDATORY SECURITY NOTICE:** Event-driven examples may reference external GeoJSON, ShapeData, or tile sources for illustration only. In production you MUST not fetch untrusted geodata directly in the client. Instead, host assets locally or serve them via a server-side ingestion pipeline that validates domains, requires HTTPS, validates GeoJSON schema, enforces size/feature limits, sanitizes/HTML-encodes properties, and records an audit with human review before any automated processing. NEVER forward raw ShapeData, tooltip, or annotation content to automated agents or LLM prompts without strict validation and sign-off. See [readme-security](readme-security.md) for required validation templates.

## Table of Contents

- [Mouse Events](#mouse-events)
   - [OnMouseMove](#onmousemove)
   - [OnMarkerClick](#onmarkerclick)
   - [OnShapeSelected](#onshapeselected)
   - [OnDoubleClick](#ondoubleclick)
   - [ CRITICAL FIX: OnMouseMove - Wrong Event Argument Class](#-critical-fix-onmousemove---wrong-event-argument-class)
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

<SfMaps @ref="mapInstance">
    <MapsEvents MouseMove="MouseMoveHandler" />
    <MapsLayers>
        <!-- Use local or validated GeoJSON in production -->
        <MapsLayer ShapeData='new {dataOptions= "/data/world-map.json"}' TValue="string">
        </MapsLayer>
    </MapsLayers>
</SfMaps>

@code {
    private SfMaps mapInstance;
    private string MousePos = "Move mouse over map";
    private void MouseMoveHandler(MouseMoveEventArgs args)
    {
        MousePos = $"Lat: {args.Latitude:F2}, Lng: {args.Longitude:F2}";
    }
}
```

### OnMarkerClick

Triggered when user clicks a marker:

```csharp
<SfMaps>
    <MapsEvents OnMarkerClick="MarkerClickHandler" />
    <MapsLayers>
        <!-- Use local or validated GeoJSON in production -->
        <MapsLayer ShapeData='new {dataOptions= "/data/world-map.json"}' TValue="string">
            <MapsMarkerSettings>
                <MapsMarker Visible="true" DataSource="@MarkerData" TValue="City"
                    Shape="Syncfusion.Blazor.Maps.MarkerType.Circle" Width="15" Height="15">
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
    public class City
    {
        public double Latitude { get; set; }
        public double Longitude { get; set; }
    }
    public List<City> MarkerData = new List<City> {
        new City {Latitude = 35.145083,Longitude = -117.960260}
    };
}
```

**Event args:**
- `Latitude`: Marker latitude coordinate
- `Longitude`: Marker longitude coordinate

### OnShapeSelected

Triggered when user selects (clicks) a shape/polygon:

```csharp
<SfMaps @ref="mapInstance" >
    <MapsEvents OnShapeSelected="ShapeSelectHandler" />
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
<SfMaps>
    <MapsEvents OnDoubleClick="DoubleClickHandler" />
    <MapsLayers>
        <!-- Use local or validated GeoJSON in production -->
        <MapsLayer ShapeData='new {dataOptions= "/data/world-map.json"}' TValue="string">
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

### CRITICAL FIX: OnMouseMove - Wrong Event Argument Class

** INCORRECT (Currently Documented):**
```csharp
private void MouseMoveHandler(MapClickEventArgs args) // WRONG CLASS!
{
    // MapClickEventArgs is for click events, not mouse move
}
```

** CORRECT:**
```csharp
@page "/mouse-move"
@using Syncfusion.Blazor.Maps

<SfMaps>
    <MapsEvents OnMouseMove="MouseMoveHandler" />
    <MapsLayers>
        <!-- Use local or validated GeoJSON in production -->
        <MapsLayer ShapeData='new {dataOptions= "/data/world-map.json"}' TValue="string">
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
<SfMaps>
    <MapsEvents OnMarkerMouseLeave="MarkerMouseLeaveHandler" />
    <MapsLayers>
        <!-- Use local or validated GeoJSON in production -->
        <MapsLayer ShapeData='new {dataOptions= "/data/world-map.json"}' TValue="string">
            <MapsMarkerSettings>
                <MapsMarker Visible="true" DataSource="@Markers" TValue="MarkerData"
                            Shape="Syncfusion.Blazor.Maps.MarkerType.Circle" Width="15" Height="15">
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
<SfMaps>
    <MapsEvents MarkerClusterClick="ClusterClickHandler" />
    <MapsLayers>
        <!-- Use local or validated GeoJSON in production -->
        <MapsLayer ShapeData='new {dataOptions= "/data/world-map.json"}' TValue="string">
            <MapsMarkerSettings>
                <MapsMarker Visible="true" DataSource="@LargeMarkerSet" TValue="City"
                            Shape="Syncfusion.Blazor.Maps.MarkerType.Circle" Width="15" Height="15">
                </MapsMarker>
            </MapsMarkerSettings>
            <MapsMarkerClusterSettings AllowClustering="true" 
                                       Shape="Syncfusion.Blazor.Maps.MarkerType.Circle" 
                                       Fill="#008CFF" Height="25" Width="25">
            </MapsMarkerClusterSettings>
        </MapsLayer>
    </MapsLayers>
</SfMaps>

@code {
    private void ClusterClickHandler(MarkerClusterClickEventArgs args)
    {
        Console.WriteLine($"Cluster clicked at ({args.Latitude}, {args.Longitude})");
        Console.WriteLine($"Data: {string.Join(", ", args.Data)}");
    }
    public class City
    {
        public double Latitude { get; set; }
        public double Longitude { get; set; }
        public string Name { get; set; }
        public double Area { get; set; }
    }
    private List<City> LargeMarkerSet = new List<City> {
        new City { Latitude=40.6971494, Longitude= -74.2598747, Name="New York", Area=8683 },
        new City { Latitude=40.0024137, Longitude= -75.2581194, Name="Philadelphia", Area=4661 },
        new City { Latitude=42.3142647, Longitude= -71.11037, Name="Boston", Area=4497 },
        new City { Latitude=42.3526257, Longitude= -83.239291, Name="Detroit", Area=3267 },
        new City { Latitude=47.2510905, Longitude= -123.1255834, Name="Washington", Area=2996 },
        new City { Latitude=25.7823907, Longitude= -80.2994995, Name="Miami", Area=2891 },
        new City { Latitude=19.3892246, Longitude= -70.1305136, Name="San Juan", Area=2309 }
    };
}
```

**Event Args Properties:**
- `Latitude`: Cluster center latitude
- `Longitude`: Cluster center longitude
- `Data`: Array of clustered marker data

---

### OnMarkerClusterMove

Triggered when mouse moves over a cluster (NEW - Previously Missing):

```csharp
<SfMaps>
    <MapsEvents MarkerClusterMouseMove="ClusterMoveHandler" />
    <MapsLayers>
        <!-- Use local or validated GeoJSON in production -->
        <MapsLayer ShapeData='new {dataOptions= "/data/world-map.json"}' TValue="string">
            <MapsMarkerSettings>
                <MapsMarker Visible="true" DataSource="@LargeMarkerSet" TValue="City"
                            Shape="Syncfusion.Blazor.Maps.MarkerType.Circle" Width="15" Height="15">
                </MapsMarker>
            </MapsMarkerSettings>
            <MapsMarkerClusterSettings AllowClustering="true" 
                                       Shape="Syncfusion.Blazor.Maps.MarkerType.Circle" 
                                       Fill="#008CFF" Height="25" Width="25">
            </MapsMarkerClusterSettings>
        </MapsLayer>
    </MapsLayers>
</SfMaps>

@code {
    private void ClusterMoveHandler(MarkerClusterMoveEventArgs args)
    {
        Console.WriteLine($"Cluster clicked at ({args.Latitude}, {args.Longitude})");
        Console.WriteLine($"Data: {string.Join(", ", args.Data)}");
        // Handle cluster interaction - expand, show details, etc.
    }
    public class City
    {
        public double Latitude { get; set; }
        public double Longitude { get; set; }
        public string Name { get; set; }
        public double Area { get; set; }
    }
    private List<City> LargeMarkerSet = new List<City> {
        new City { Latitude=40.6971494, Longitude= -74.2598747, Name="New York", Area=8683 },
        new City { Latitude=40.0024137, Longitude= -75.2581194, Name="Philadelphia", Area=4661 },
        new City { Latitude=42.3142647, Longitude= -71.11037, Name="Boston", Area=4497 },
        new City { Latitude=42.3526257, Longitude= -83.239291, Name="Detroit", Area=3267 },
        new City { Latitude=47.2510905, Longitude= -123.1255834, Name="Washington", Area=2996 },
        new City { Latitude=25.7823907, Longitude= -80.2994995, Name="Miami", Area=2891 },
        new City { Latitude=19.3892246, Longitude= -70.1305136, Name="San Juan", Area=2309 }
    };
}
```

## Pan and Zoom Events

### OnZoomChange

Triggered when zoom level changes (user zoom, programmatic zoom):

```csharp
@page "/zoom-event"
@using Syncfusion.Blazor.Maps

<p>Current zoom level: @CurrentZoomLevel</p>

<SfMaps>
    <MapsEvents OnZoom="ZoomChangeHandler"/>
    <MapsZoomSettings Enable="true"></MapsZoomSettings>
    <MapsLayers>
        <!-- Use local or validated GeoJSON in production -->
        <MapsLayer ShapeData='new {dataOptions= "/data/world-map.json"}' TValue="string">
        </MapsLayer>
    </MapsLayers>
</SfMaps>

@code {
    private int CurrentZoomLevel = 1;
    private void ZoomChangeHandler(MapZoomEventArgs args)
    {
        CurrentZoomLevel = (int)args.Scale;
        Console.WriteLine($"Zoomed to level: {args.Scale}");
        // Load new data, adjust display, etc.
    }
}
```

**Event args:**
- `TranslatePoint`: The new translate point
- `Scale`: The previous zoom level
- `Type`: "ZoomIn", "ZoomOut", or programmatic

### OnPan

Triggered when map is panned:

```csharp
<SfMaps>
    <MapsEvents OnPan="PanHandler" />
    <MapsZoomSettings Enable="true"></MapsZoomSettings>
    <MapsLayers>
        <!-- Use local or validated GeoJSON in production -->
        <MapsLayer ShapeData='new {dataOptions= "/data/world-map.json"}' TValue="string">
        </MapsLayer>
    </MapsLayers>
</SfMaps>

@code {
    private void PanHandler(MapPanEventArgs args)
    {
        Console.WriteLine($"Panned to: Lat {args.Latitude}, Lng {args.Longitude}");
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
    <MapsEvents ShapeRendering="@ShapeRenderingEvent"></MapsEvents>
    <MapsLayers>
        <!-- Use local or validated GeoJSON in production -->
        <MapsLayer ShapeData='new {dataOptions= "/data/world-map.json"}' TValue="string">
        </MapsLayer>
    </MapsLayers>
</SfMaps>

@code {
    public void ShapeRenderingEvent(Syncfusion.Blazor.Maps.ShapeRenderingEventArgs args)
    {
        // Here you can customize your code
    }
}
```

**Event Args:**
- `Data`: The shape's data object
- `Fill`: Customize shape fill color (get/set)
- `Index`: Customize layer index (get/set)

---

### OnLayerRendering

Triggered before each layer is rendered (NEW - Previously Underdocumented):

```csharp
<SfMaps>
    <MapsEvents OnLayerRendering="OnLayerRendering">
    </MapsEvents>
        <MapsLayers>
        <MapsLayer ShapeData='new {dataOptions= "/data/world-map.json"}' TValue="string">
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
    <MapsEvents AnnotationRendering="OnAnnotationRendering"/>
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
        <MapsLayer ShapeData='new {dataOptions= "/data/world-map.json"}' TValue="string">
        </MapsLayer>
    </MapsLayers>
</SfMaps>

@code {
    private void OnAnnotationRendering(AnnotationRenderingEventArgs args)
    {
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
@using Syncfusion.Blazor.Maps

<button @onclick="ZoomToCoordinates">ZoomToCoordinates</button>
<SfMaps @ref="maps">
    <MapsZoomSettings Enable="true">
    </MapsZoomSettings>
    <MapsLayers>
        <MapsLayer ShapeData='new {dataOptions= "/data/world-map.json"}' TValue="string">
        </MapsLayer>
    </MapsLayers>
</SfMaps>

@code {
    SfMaps maps;
    public void ZoomToCoordinates()
    {
        maps.ZoomToCoordinates(0, 0, 100, 100);
    }
}
```

---

### ZoomByPosition()

Zoom the map from a specific center position with a zoom factor:

```csharp
@using Syncfusion.Blazor.Maps

<button @onclick="ZoomByPosition">ZoomByPosition</button>
<SfMaps @ref="maps">
    <MapsZoomSettings Enable="true">
    </MapsZoomSettings>
    <MapsLayers>
        <MapsLayer ShapeData='new {dataOptions= "/data/world-map.json"}' TValue="string">
        </MapsLayer>
    </MapsLayers>
</SfMaps>

@code {
    SfMaps maps;

    public void ZoomByPosition()
    {
        MapsCenterPosition centerPosition = new MapsCenterPosition();
        centerPosition.Latitude = 35.145083;
        centerPosition.Longitude = -117.960260;
        maps.ZoomByPosition(centerPosition, 2);
    }
}
```

---

### GetMinMaxLatitudeLongitude()

Get the minimum and maximum coordinates of the visible map area:

```csharp
@using Syncfusion.Blazor.Maps
@using System.Collections.ObjectModel;

<button @onclick="GetMinMaxLatitudeLongitude">GetMinMaxLatitudeLongitude</button>

@if(MapBoundCoordinates != null)
{
    <div>
        Maximum Latitude = @MapBoundCoordinates.MaxLatitude <br/>
        Minimum Latitude = @MapBoundCoordinates.MinLatitude  <br />
        Maximum Longitude = @MapBoundCoordinates.MaxLongitude <br />
        Minimum Longitude = @MapBoundCoordinates.MinLongitude
    </div>
}

<SfMaps ID="maps" @ref="MapsRef">
    <MapsZoomSettings Enable="true" ZoomFactor="@ZoomFactor"></MapsZoomSettings>
    <MapsCenterPosition Latitude="@CenterLat" Longitude="@CenterLong"></MapsCenterPosition>
    <MapsLayers>
        <!-- Use local or validated GeoJSON in production -->
        <MapsLayer ShapeData='new {dataOptions= "/data/world-map.json"}' TValue="string">
            <MapsMarkerSettings>
                <MapsMarker Visible="true" DataSource="MarkerDataSource" Height="25" Width="25" TValue="MarkerData" Shape="Syncfusion.Blazor.Maps.MarkerType.Circle" AnimationDuration="1500">
                </MapsMarker>
            </MapsMarkerSettings>
        </MapsLayer>
    </MapsLayers>
</SfMaps>


@code {
    SfMaps MapsRef;
    public double ZoomFactor = 7;
    public double CenterLat = 21.815447;
    public double CenterLong = 80.1932;
    public MinMaxLatitudeLongitude MapBoundCoordinates;

    public class MarkerData
    {
        public string Name{ get; set; }
        public double Latitude { get; set; }
        public double Longitude { get; set; }
    }

    public void GetMinMaxLatitudeLongitude()
    {
        MapBoundCoordinates = MapsRef?.GetMinMaxLatitudeLongitude();
    }

    public ObservableCollection<MarkerData> MarkerDataSource = new ObservableCollection<MarkerData> {
        new MarkerData {Latitude=22.572646,Longitude=88.363895},
        new MarkerData {Latitude=25.0700428,Longitude=67.2847875}
    };
}
```

**Note:** This is a synchronous method.

---

### ShapeSelectionAsync()

Programmatically select or unselect shapes in the map:

```csharp
@using Syncfusion.Blazor.Maps

<button @onclick="ShapeSelectAsync">Select Shape</button>
<button @onclick="DeselectShape">Deselect Shape</button>
<SfMaps @ref="maps">
    <MapsZoomSettings Enable="true" EnablePanning="true">
    </MapsZoomSettings>
    <MapsLayers>
        <MapsLayer ShapeData='new {dataOptions= "/data/world-map.json"}' TValue="string">
            <MapsLayerSelectionSettings Enable="true" Fill="Green"></MapsLayerSelectionSettings>
        </MapsLayer>
    </MapsLayers>
</SfMaps>

@code {
    SfMaps maps;

    public async Task ShapeSelectAsync()
    {
         await maps.ShapeSelectionAsync(0, "name", "Argentina");
    }
    public async Task DeselectShape()
    {
         await maps.ShapeSelectionAsync(0, "name", "Argentina");
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

<SfMaps @ref="mapInstance">
    <MapsEvents MouseMove="MouseMoveHandler"
    OnMarkerClick="MarkerClickHandler"
    ShapeSelected="ShapeSelectHandler" />
    <MapsLayers>
        <MapsLayer ShapeData='new {dataOptions= "/data/world-map.json"}' TValue="string">
            <MapsMarkerSettings>
                <MapsMarker Visible="true" DataSource="@MarkerData" TValue="City">
                </MapsMarker>
            </MapsMarkerSettings>
        </MapsLayer>
    </MapsLayers>
</SfMaps>

@code {
    private SfMaps mapInstance;
    public class City
    {
        public double Latitude { get; set; }
        public double Longitude { get; set; }
    }

    public List<City> MarkerData = new List<City> {
        new City {Latitude = 35.145083,Longitude = -117.960260}
    };
    private string LastEventType = "None";
    private string EventDetails = "";
    private void MouseMoveHandler(MouseMoveEventArgs args)
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

<SfMaps>
    <MapsEvents OnMarkerClick="MarkerClickHandler" />
    <MapsLayers>
        <MapsLayer ShapeData='new {dataOptions= "/data/world-map.json"}' TValue="string">
            <MapsMarkerSettings>
                <MapsMarker Visible="true" DataSource="@MarkerData" TValue="City">
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
    }

    public List<City> MarkerData = new List<City> {
        new City {Latitude=37.368, Longitude=-122.095},
        new City { Latitude = 40.724546, Longitude = -73.850344 }
    };
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

## SfMaps Methods API Reference

### Pan Methods

```csharp
// Pan in specific direction
// Direction values: Bottom, Left, Right, Top
Syncfusion.Blazor.Maps.Internal.Point position = new Syncfusion.Blazor.Maps.Internal.Point();
position.X = 120;
position.Y = 200;
mapInstance.PanByDirectionAsync(PanDirection.Bottom, position);
```

### Refresh

```csharp
// Refresh map rendering (updates all layers, markers, data bindings)
mapInstance.Refresh();

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
MinMaxLatitudeLongitude bounds = mapInstance.GetMinMaxLatitudeLongitude();
// Returns: MinLatitude, MaxLatitude, MinLongitude, MaxLongitude
```

## Events API Reference

### Complete Event Binding Example

```csharp
<MapsEvents OnMarkerClick="MarkerClickHandler"
            OnMarkerDragStart="DragStartHandler"
            OnMarkerDragEnd="DragEndHandler"
            ShapeSelected="ShapeSelectHandler"
            OnZoom="ZoomChangeHandler"
            OnPan="PanHandler"
            OnLoad="LoadHandler"
            Loaded="LoadedHandler"
            MouseMove="MouseMoveHandler"
            OnDoubleClick="DoubleClickHandler"
            OnPrint="PrintHandler"
            Resizing="ResizeHandler">
</MapsEvents>
```

### Available Events

| Event | Arguments | Trigger |
|-------|-----------|---------|
| `OnMarkerClick` | `MarkerClickEventArgs` | Marker clicked |
| `OnMarkerDragStart` | `MarkerDragStartEventArgs` | Marker drag begins |
| `OnMarkerDragEnd` | `MarkerDragEndEventArgs` | Marker drag ends |
| `MarkerMove` | `MarkerMoveEventArgs` | Marker moving (dragging) |
| `OnMarkerMouseLeave` | `MarkerMouseLeaveEventArgs` | Mouse leaves marker |
| `OnBubbleClick` | `BubbleClickEventArgs` | Bubble clicked |
| `OnBubbleMouseMove` | `BubbleMoveEventArgs` | Mouse moves over bubble |
| `ShapeSelected` | `ShapeSelectedEventArgs` | Shape/polygon selected |
| `OnZoom` | `MapZoomEventArgs` | Zoom level changed |
| `OnPan` | `MapPanEventArgs` | Map panned |
| `MouseMove` | `MouseMoveEventArgs` | Mouse moves on map |
| `OnDoubleClick` | `MouseEventArgs` | Double-click on map |
| `OnLoad` | `LoadEventArgs` | Before map loads |
| `Loaded` | `LoadedEventArgs` | After map loads |
| `ShapeRendering` | `ShapeRenderingEventArgs` | Before shape renders |
| `MarkerRendering` | `MarkerRenderingEventArgs` | Before marker renders |
| `BubbleRendering` | `BubbleRenderingEventArgs` | Before bubble renders |
| `LabelRendering` | `LabelRenderingEventArgs` | Before label renders |
| `LayerRendering` | `LayerRenderingEventArgs` | Before layer renders |
| `LegendRendering` | `LegendRenderingEventArgs` | Before legend renders |
| `OnPrint` | `PrintEventArgs` | Before printing |
| `Resizing` | `ResizeEventArgs` | Map resized |
| `AnimationCompleted` | `AnimationCompleteEventArgs` | Animation finished |

### Event Arguments Details

```csharp
// MarkerClickEventArgs
public class MarkerClickEventArgs
{
    public double Latitude { get; set; }
    public double Longitude { get; set; }
    public Dictionary<string, string> Data { get; set; }
    public string Target { get; set; }
    public double X { get; set; }
    public double Y { get; set; }
    public bool IsTouch { get; set; }
    public string Value { get; set; }
}

// MapZoomEventArgs
public class MapZoomEventArgs
{
    public double PreviousZoomLevel { get; set; }
    public double NewZoomLevel { get; set; }
    public string Type { get; set; }  // "ZoomIn", "ZoomOut"
}

// MapPanEventArgs
public class MapZoomEventArgs
{
    public double Scale { get; set; }
    public string Type { get; set; }  // "ZoomIn", "ZoomOut", or programmatic
    public PointF TileTranslatePoint { get; set; }
    public double TileZoomLevel { get; set; }
    public PointF TranslatePoint { get; set; }
}

// ShapeSelectedEventArgs
public class ShapeSelectedEventArgs
{
    public MapsBorderSettings Border { get; set; }
    public Dictionary<string, string> Data { get; set; }
    public string Fill { get; set; }
    public double Opacity { get; set; }
    public Dictionary<string, string> ShapeData { get; set; }
    public string Target { get; set; }
}
```

For complete event arguments, see [references/api-reference.md](api-reference.md#event-arguments)


