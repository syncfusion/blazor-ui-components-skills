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

# User Interactions

## Handling Mouse Clicks

### Click on Markers

```csharp
@page "/marker-click"
@using Syncfusion.Blazor.Maps

<div>Last clicked: @ClickedMarkerInfo</div>

<SfMaps @ref="mapInstance" OnMarkerClick="MarkerClickHandler">
    <MapsLayers>
        <MapsLayer UrlTemplate="https://tile.openstreetmap.org/level/tileX/tileY.png" TValue="string">
            <MapsMarkerSettings>
                <MapsMarker Latitude="37.368" Longitude="-122.095" 
                    Shape="MarkerType.Circle" Width="15" Height="15">
                </MapsMarker>
                <MapsMarker Latitude="40.7128" Longitude="-74.0060" 
                    Shape="MarkerType.Circle" Width="15" Height="15">
                </MapsMarker>
            </MapsMarkerSettings>
        </MapsLayer>
    </MapsLayers>
</SfMaps>

@code {
    private SfMaps mapInstance;
    private string ClickedMarkerInfo = "None";

    private void MarkerClickHandler(MarkerClickEventArgs args)
    {
        ClickedMarkerInfo = $"Latitude: {args.Latitude}, Longitude: {args.Longitude}";
    }
}
```

### Click on Shapes (Polygons)

```csharp
<SfMaps @ref="mapInstance" OnShapeSelected="ShapeClickHandler">
    <MapsLayers>
        <MapsLayer TValue="StateData" DataSource="@StateData">
            <MapsShapeSettings Fill="lightblue">
            </MapsShapeSettings>
        </MapsLayer>
    </MapsLayers>
</SfMaps>

@code {
    private SfMaps mapInstance;

    private void ShapeClickHandler(ShapeSelectedEventArgs args)
    {
        Console.WriteLine($"Selected shape data: {args.ShapeData}");
    }
}
```

### Double-Click Events

```csharp
<SfMaps @ref="mapInstance" OnDoubleClick="DoubleClickHandler">
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
    private void DoubleClickHandler(MapClickEventArgs args)
    {
        Console.WriteLine($"Double-clicked at: {args.Latitude}, {args.Longitude}");
    }
}
```

## Zoom and Pan Controls

### Enable/Disable Zoom

```csharp
<SfMaps ZoomSettings="@ZoomOptions">
    <MapsLayers>
        <MapsLayer UrlTemplate="https://tile.openstreetmap.org/level/tileX/tileY.png" TValue="string">
        </MapsLayer>
    </MapsLayers>
</SfMaps>

@code {
    private MapsZoomSettings ZoomOptions = new()
    {
        Enable = true,
        ZoomFactor = 2,  // Multiplier for each zoom action
        MaxZoom = 20,
        MinZoom = 1
    };
}
```

### Programmatic Zoom

```csharp
@page "/zoom-control"
@using Syncfusion.Blazor.Maps


<SfMaps @ref="mapInstance" >
<MapsZoomSettings ZoomFactor="4"></MapsZoomSettings>
    <MapsLayers>
        <MapsLayer UrlTemplate="https://tile.openstreetmap.org/level/tileX/tileY.png" TValue="string">
        </MapsLayer>
    </MapsLayers>
</SfMaps>

@code {
    private SfMaps mapInstance;
    private MapsCenterPosition CenterLatLng = new MapsCenterPosition { Latitude = 39.0, Longitude = -98.0 };

    
}
```

## Tooltip and Popup Handling

### Basic Tooltip

```csharp
<SfMaps>
    <MapsLayers>
        <MapsLayer UrlTemplate="https://tile.openstreetmap.org/level/tileX/tileY.png" TValue="string">
            <MapsMarkerSettings>
                <MapsMarker Latitude="37.368" Longitude="-122.095"
                    Tooltip="San Francisco, CA">
                </MapsMarker>
            </MapsMarkerSettings>
        </MapsLayer>
    </MapsLayers>
</SfMaps>
```

### Tooltip on Hover

```csharp
<SfMaps>
    <MapsLayers>
        <MapsLayer TValue="string">
            <MapsShapeSettings Fill="lightblue">
                <MapsShapeTooltipSettings Visible="true" 
                    ValuePath="StateName">
                </MapsShapeTooltipSettings>
            </MapsShapeSettings>
        </MapsLayer>
    </MapsLayers>
</SfMaps>
```

When user hovers over a shape, the tooltip displays the value from `StateName` property.

### Custom Popup on Click

```csharp
@page "/popup-map"
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
}
```

## Keyboard Navigation

### Keyboard Controls

Enable keyboard navigation for accessibility:

```csharp
<SfMaps NavigationButtons="true">
    <MapsLayers>
        <MapsLayer UrlTemplate="https://tile.openstreetmap.org/level/tileX/tileY.png" TValue="string">
        </MapsLayer>
    </MapsLayers>
</SfMaps>
```

**Keyboard shortcuts:**
- `+` : Zoom in
- `-` : Zoom out
- `Arrow keys`: Pan map
- `Home`: Reset to default view

### Custom Keyboard Handlers

```csharp
@page "/keyboard-map"
@using Syncfusion.Blazor.Maps

<SfMaps @ref="mapInstance" @onkeydown="HandleKeyDown" 
    <SfMaps @ref="mapInstance" tabindex="0" >
    <MapsZoomSettings ZoomFactor="4"></MapsZoomSettings>
    <MapsLayers>
        <MapsLayer UrlTemplate="https://tile.openstreetmap.org/level/tileX/tileY.png" TValue="string">
        </MapsLayer>
    </MapsLayers>
</SfMaps>

<p>Use arrow keys to pan, +/- to zoom</p>

@code {
    private SfMaps mapInstance;
    private MapsCenterPosition CenterLatLng = new MapsCenterPosition { Latitude = 39.0, Longitude = -98.0 };

}
```

## Custom Interaction Patterns

### Selection State Management

Track which elements are selected:

```csharp
@page "/selection-tracking"
@using Syncfusion.Blazor.Maps

<div>Selected markers: @string.Join(", ", SelectedMarkers.Select(m => $"({m.Lat},{m.Lng})"))</div>

<SfMaps @ref="mapInstance" OnMarkerClick="MarkerClickHandler">
    <MapsLayers>
        <MapsLayer UrlTemplate="https://tile.openstreetmap.org/level/tileX/tileY.png" TValue="string">
            <MapsMarkerSettings>
                @foreach (var marker in AllMarkers)
                {
                    var isSelected = SelectedMarkers.Any(m => m.Lat == marker.Lat && m.Lng == marker.Lng);
                    <MapsMarker Latitude="@marker.Lat" Longitude="@marker.Lng"
                        Fill="@(isSelected ? "red" : "blue")"
                        Shape="MarkerType.Circle" Width="15" Height="15">
                    </MapsMarker>
                }
            </MapsMarkerSettings>
        </MapsLayer>
    </MapsLayers>
</SfMaps>

@code {
    private SfMaps mapInstance;
    private List<MarkerData> AllMarkers = new()
    {
        new MarkerData { Lat = 37.368, Lng = -122.095 },
        new MarkerData { Lat = 40.7128, Lng = -74.0060 }
    };
    private List<MarkerData> SelectedMarkers = new();

    private void MarkerClickHandler(MarkerClickEventArgs args)
    {
        var marker = new MarkerData { Lat = args.Latitude, Lng = args.Longitude };
        if (SelectedMarkers.Any(m => m.Lat == marker.Lat && m.Lng == marker.Lng))
        {
            SelectedMarkers.Remove(marker);
        }
        else
        {
            SelectedMarkers.Add(marker);
        }
    }

    public class MarkerData
    {
        public double Lat { get; set; }
        public double Lng { get; set; }

        public override bool Equals(object obj)
        {
            if (obj is MarkerData other)
                return Lat == other.Lat && Lng == other.Lng;
            return false;
        }

        public override int GetHashCode()
        {
            return HashCode.Combine(Lat, Lng);
        }
    }
}
```

### Drag to Pan

Most maps support native drag-to-pan. Enable explicitly:

```csharp
<SfMaps AllowDrag="true">
    <MapsLayers>
        <MapsLayer UrlTemplate="https://tile.openstreetmap.org/level/tileX/tileY.png" TValue="string">
        </MapsLayer>
    </MapsLayers>
</SfMaps>
```

Users can now click and drag the map to pan smoothly.


