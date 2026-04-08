# API Reference - Syncfusion Maps for Blazor

Complete API reference for the Syncfusion Blazor Maps component, including all classes, interfaces, enums, and events.

> **MANDATORY SECURITY NOTICE:** Do NOT load GeoJSON, ShapeData, tile, or image resources directly from untrusted third‑party URLs at runtime. Host assets locally or return server-validated, signed URLs; validate GeoJSON/ShapeData against a strict schema, sanitize/HTML-encode properties, enforce size/complexity limits, and require human review before automated processing or forwarding to agents.

## Table of Contents

- [Main Component](#main-component)
   - [SfMaps](#sfmaps)
- [Core Components](#core-components)
   - [MapsMarker<TValue>](#mapsmarkertvalue)
   - [MapsLayer<TValue>](#mapslayertvalue)
- [Configuration Classes](#configuration-classes)
   - [MapsMargin](#mapsmargin)
   - [MapsTitleSettings](#mapstitlesettings)
   - [MapsSubtitleSettings](#mapssubtitlesettings)
   - [MapsLegendSettings](#mapslegendsettings)
   - [MapsZoomSettings](#mapszoomsettings)
   - [MapsAreaSettings](#mapsareasettings)
   - [MapsSelectionSettings](#mapsselectionsettings)
   - [MapsHighlightSettings](#mapshighlightsettings)
   - [MapsTooltipSettings](#mapstooltipsettings)
   - [MapsMarkerSettings](#mapsmarkersettings)
   - [MapsMarkerClusterSettings](#mapsmarkerclustersettings)
   - [MapsBubbleSettings](#mapsbubblesettings)
   - [MapsShapeSettings](#mapsshapesettings)
   - [MapsDataLabelSettings](#mapsdatalabelsettings)
   - [MapsAnnotation](#mapsannotation)
   - [MapsNavigationLine](#mapsnavigationline)
   - [MapsPolygon](#mapspolygon)
- [Event Arguments](#event-arguments)
   - [MarkerClickEventArgs](#markerclickeventargs)
   - [MarkerDragStartEventArgs](#markerdragstarteventargs)
   - [MarkerDragEndEventArgs](#markerdragendeventargs)
   - [MarkerMoveEventArgs](#markermoveeventargs)
   - [BubbleClickEventArgs](#bubbleclickeventargs)
   - [ShapeSelectedEventArgs](#shapeselectedeventargs)
   - [SelectionEventArgs](#selectioneventargs)
   - [MapZoomEventArgs](#mapzoomeventargs)
   - [MapPanEventArgs](#mappaneventargs)
   - [LabelRenderingEventArgs](#labelrenderingeventargs)
   - [LayerRenderingEventArgs](#layerrenderingeventargs)
   - [ShapeRenderingEventArgs](#shaperenderingeventargs)
   - [MarkerRenderingEventArgs](#markerrenderingeventargs)
   - [BubbleRenderingEventArgs](#bubblerenderingeventargs)
   - [AnimationCompleteEventArgs](#animationcompleteeventargs)
   - [TooltipRenderEventArgs](#tooltiprendereventargs)
   - [LegendRenderingEventArgs](#legendrenderingeventargs)
   - [PrintEventArgs](#printeventargs)
   - [ResizeEventArgs](#resizeeventargs)
   - [LoadEventArgs](#loadeventargs)
   - [LoadedEventArgs](#loadedeventargs)
   - [AnnotationRenderingEventArgs](#annotationrenderingeventargs)
- [Interfaces](#interfaces)
   - [ILayer](#ilayer)
   - [IMarker](#imarker)
   - [IBubble](#ibubble)
- [Enums](#enums)
   - [MarkerType](#markertype)
   - [ExportType](#exporttype)
   - [ProjectionType](#projectiontype)
   - [GeometryType](#geometrytype)
   - [LegendPosition](#legendposition)
   - [LegendMode](#legendmode)
   - [LegendType](#legendtype)
   - [LegendArrangement](#legendarrangement)
   - [LegendShape](#legendshape)
   - [Alignment](#alignment)
   - [Orientation](#orientation)
   - [TooltipGesture](#tooltipgesture)
   - [BubbleType](#bubbletype)
   - [SmartLabelMode](#smartlabelmode)
   - [IntersectAction](#intersectaction)
   - [LabelPosition](#labelposition)
   - [ArrowPosition](#arrowposition)
   - [PolygonShapeType](#polygonshapetype)
   - [PanDirection](#pandirection)
   - [ToolbarItem](#toolbaritem)
   - [Type](#type)
- [Properties Quick Reference](#properties-quick-reference)
   - [Map Container Properties](#map-container-properties)
   - [Interactive Properties](#interactive-properties)
   - [Display Properties](#display-properties)
   - [Marker Properties](#marker-properties)
   - [Shape Properties](#shape-properties)


## Main Component

### SfMaps

The primary Maps component for rendering interactive geospatial visualizations.

**Namespace:** `Syncfusion.Blazor.Maps`

**Basic Usage:**
```csharp
@using Syncfusion.Blazor.Maps
<SfMaps @ref="mapInstance">
    <MapsEvents ShapeSelected="@ShapeSelectedEvent"></MapsEvents>
    <MapsLayers>
        <!-- In production, bundle GeoJSON locally or validate external sources. Example uses local path: /data/world-map.json -->
        <MapsLayer ShapeData='new {dataOptions= "/data/world-map.json"}' TValue="string">
            <MapsLayerSelectionSettings Enable="true" Fill="green">
                <MapsLayerSelectionBorder Color="White" Width="2"></MapsLayerSelectionBorder>
            </MapsLayerSelectionSettings>
        </MapsLayer>
    </MapsLayers>
</SfMaps>

@code {
    SfMaps mapInstance;
    public void ShapeSelectedEvent(Syncfusion.Blazor.Maps.ShapeSelectedEventArgs args)
    {
        // Here you can customize your code
    }
}
```

**Key Properties:**

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `ZoomLevel` | `int` | 1 | Initial zoom level (1-20) |
| `Width` | `string` | "100%" | Container width |
| `Height` | `string` | "400px" | Container height |
| `Margin` | `MapsMargin` | null | Map margins (Top, Bottom, Left, Right) |
| `Orientation` | `Orientation` | Orientation.Portrait | Layout orientation |
| `BaseLayerIndex` | `int` | 0 | Index of base tile layer |
| `EnableLegend` | `bool` | true | Show/hide legend |
| `EnableNavigation` | `bool` | false | Show/hide navigation lines |
| `AllowImageExport` | `bool` | true | Enable PNG/SVG export functionality |
| `AllowPdfExport` | `bool` | true | Enable PDF export functionality |
| `AllowPrint` | `bool` | true | Enable map printing |
| `EnablePersistence` | `bool` | false | Save and restore map state (zoom, pan, selection) |
| `ProjectionType` | `ProjectionType` | Mercator | Map projection type (Mercator, EquirectangularProjection, GnomnicProjection, PolyconicProjection, MercatorSpherical) |
| `Theme` | `string` | "Material" | Visual theme (Material, Fabric, Bootstrap, Bootstrap4, Bootstrap5, HighContrast, FluentDark) |
| `TitleSettings` | `MapsTitleSettings` | null | Title configuration |
| `SubtitleSettings` | `MapsSubtitleSettings` | null | Subtitle configuration |
| `LegendSettings` | `MapsLegendSettings` | null | Legend configuration |
| `ZoomSettings` | `MapsZoomSettings` | null | Zoom behavior configuration |
| `AreaSettings` | `MapsAreaSettings` | null | Map area/background settings |
| `SelectionSettings` | `MapsSelectionSettings` | null | Shape/marker selection behavior |
| `HighlightSettings` | `MapsHighlightSettings` | null | Highlight behavior on hover |
| `TooltipSettings` | `MapsTooltipSettings` | null | Tooltip configuration |
| `Locale` | `string` | "en-US" | Localization identifier |
| `TabIndex` | `int` | 0 | Tab index for keyboard navigation |
| `Background` | `string` | "white" | Map background color |
| `Format` | `string` | null | Number/date format strings for data binding |
| `EnableGroupingSeparator` | `bool` | false | Add thousands separator to numbers |
| `Description` | `string` | null | ARIA description for accessibility |

**Methods:**

```csharp
// ✅ Refresh map rendering
mapInstance.Refresh();

// ✅ Zoom to coordinates region (Zoom to fit specific area)
await mapInstance.ZoomToCoordinates(35, -120, 40, -115); // minLat, minLng, maxLat, maxLng

// ✅ Get min/max coordinates of visible area
var visibleBounds = mapInstance.GetMinMaxLatitudeLongitude();
Console.WriteLine($"Visible area: Lat {visibleBounds.MinLatitude} to {visibleBounds.MaxLatitude}");

// ✅ Zoom by position factor (Zoom in/out from specific point)
await mapInstance.ZoomByPosition(new MapsCenterPosition { Latitude = 37, Longitude = -122 }, 1.5);

// ✅ Programmatically select shapes
await mapInstance.ShapeSelectionAsync(layerIndex: 0, propertyName: "name", name: "California", enable: true);

// ✅ Export map as image
await mapInstance.ExportAsync(ExportType.PNG, "map-export");

// ✅ Export map as SVG
await mapInstance.ExportAsync(ExportType.SVG, "map-export");

// ✅ Export map as PDF with orientation
await mapInstance.ExportAsync(ExportType.PDF, "map-export", PdfPageOrientation.Portrait);
await mapInstance.ExportAsync(ExportType.PDF, "map-export", PdfPageOrientation.Landscape);

// ✅ Print map
await mapInstance.PrintAsync();

// ✅ Get Bing Maps URL template (Static method)
string bingUrlTemplate = await SfMaps.GetBingUrlTemplate("BingMapsServiceLink_Key");
```

**Export and Print Requirements:**
- Ensure `AllowImageExport="true"` for PNG/SVG export
- Ensure `AllowPdfExport="true"` for PDF export
- Ensure `AllowPrint="true"` for print functionality

---

## Core Components

### MapsMarker<TValue>

Generic marker component for rendering markers on the map with support for data binding.

**Namespace:** `Syncfusion.Blazor.Maps`

**Basic Usage:**
```csharp
<MapsMarker DataSource="MarkerDataSource"
            TValue="City"
            Shape="MarkerType.Circle"
            Width="20" 
            Height="20"
            Fill="red"
            Opacity="0.8"
            Visible="true"
            EnableDrag="false">
</MapsMarker>

@code {
    public class City
    {
        public double Latitude { get; set; }
        public double Longitude { get; set; }
    }
    public List<City> MarkerDataSource = new List<City> {
    };
}
```

**Key Properties:**

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `Shape` | `MarkerType` | Balloon | Marker shape (Circle, Rectangle, Triangle, Cross, Diamond, Image, Balloon) |
| `Latitude` | double | - | Y-coordinate (-90 to 90) |
| `Longitude` | double | - | X-coordinate (-180 to 180) |
| `Width` | double | 10 | Marker width in pixels |
| `Height` | double | 10 | Marker height in pixels |
| `Fill` | string | #FF471A | Marker color (hex or named) |
| `Opacity` | double | 1 | Transparency (0-1) |
| `ImageUrl` | string | null | Custom image URL (when Shape is Image) |
| `Visible` | bool | true | Show/hide marker |
| `EnableDrag` | bool | false | Allow marker dragging |
| `AnimationDuration` | double | 1000 | Animation duration in milliseconds |
| `AnimationDelay` | double | 0 | Animation delay in milliseconds |
| `OffsetX` | double | 0 | Horizontal offset from longitude |
| `OffsetY` | double | 0 | Vertical offset from latitude |
| `DashArray` | string | null | Dash pattern for marker outline |

**Data Binding Properties:**

| Property | Type | Description |
|----------|------|-------------|
| `DataSource` | IEnumerable<object> | Collection of marker data |
| `LatitudeValuePath` | string | Field for latitude values |
| `LongitudeValuePath` | string | Field for longitude values |
| `ColorValuePath` | string | Field for dynamic marker color |
| `WidthValuePath` | string | Field for dynamic marker width |
| `HeightValuePath` | string | Field for dynamic marker height |
| `ShapeValuePath` | string | Field for dynamic marker shape |
| `ImageUrlValuePath` | string | Field for dynamic image URL |
| `LegendText` | string | Legend label text |

### MapsLayer<TValue>

Generic layer component for rendering geographic data including shapes, markers, and bubbles.

**Namespace:** `Syncfusion.Blazor.Maps`

**Key Properties:**

| Property | Type | Description |
|----------|------|-------------|
| `UrlTemplate` | string | Tile provider URL template for base maps |
| `Type` | `Type` | Layer type: GeometryNormalShape, ShapeGeometry, OSMShapeGeometry |
| `ShapeDataSource` | object | GeoJSON data for shapes and polygons |
| `DataSource` | object | Data for color/value mapping |
| `ShapePropertyPath` | string | Data field for matching shapes |
| `ShapeSettings` | `MapsShapeSettings` | Shape appearance configuration |
| `MarkerSettings` | `MapsMarkerSettings` | Marker appearance defaults |

---

## Configuration Classes

```csharp
public class MapsCenterPosition
{
    public double Latitude { get; set; }  // -90 to 90
    public double Longitude { get; set; } // -180 to 180
}
```

### MapsMargin
Defines space around map edges.

```csharp
public class MapsMargin
{
    public double Left { get; set; }
    public double Right { get; set; }
    public double Top { get; set; }
    public double Bottom { get; set; }
}
```

### MapsTitleSettings
Configures the map title.

```csharp
public class MapsTitleSettings
{
    public string Text { get; set; }
    public MapsTitleTextStyle TextStyle { get; set; }
    public Alignment Alignment { get; set; }
    public string Description { get; set; }
}
```

### MapsSubtitleSettings
Configures the map subtitle.

```csharp
public class MapsSubtitleSettings
{
    public string Text { get; set; }
    public MapsSubtitleTextStyle TextStyle { get; set; }
    public Alignment Alignment { get; set; }
    public string Description { get; set; }
}
```

### MapsLegendSettings
Controls legend appearance and behavior.

```csharp
public class MapsLegendSettings
{
    public bool Visible { get; set; }
    public LegendPosition Position { get; set; }        // Top, Bottom, Left, Right
    public LegendMode Mode { get; set; }                // Default, Interactive
    public LegendType Type { get; set; }                // Layers, Markers, Bubbles, Shapes
    public LegendArrangement Arrangement { get; set; }  // Vertical, Horizontal
    public double Width { get; set; }
    public double Height { get; set; }
    public MapsLegendBorder Border { get; set; }
    public MapsLegendTextStyle TextStyle { get; set; }
    public bool ToggleLegendVisibility { get; set; }
}
```

### MapsZoomSettings
Controls zoom behavior.

```csharp
public class MapsZoomSettings
{
    public bool EnableZoom { get; set; }              // Enable/disable zooming
    public bool EnableDynamicZoom { get; set; }       // Dynamic zoom on interaction
    public bool EnableZoomOnDoubleClick { get; set; } // Double-click to zoom
    public bool EnablePinchZooming { get; set; }      // Touch pinch zoom
    public double MinZoom { get; set; }               // Minimum zoom level
    public double MaxZoom { get; set; }               // Maximum zoom level
    public Orientation ToolbarOrientation { get; set; } // Toolbar placement
    public MapsZoomToolbarSettings ToolbarSettings { get; set; }
}
```

### MapsAreaSettings
Configures map background and borders.

```csharp
public class MapsAreaSettings
{
    public string Background { get; set; }
    public MapsAreaBorder Border { get; set; }
}
```

### MapsSelectionSettings
Controls shape/marker selection behavior.

```csharp
public class MapsSelectionSettings
{
    public bool Enable { get; set; }
    public string Fill { get; set; }
    public double Opacity { get; set; }
    public MapsSelectionSettingsBorder Border { get; set; }
    public bool EnableMultiSelect { get; set; }
}
```

### MapsHighlightSettings
Controls hover highlight behavior.

```csharp
public class MapsHighlightSettings
{
    public bool Enable { get; set; }
    public string Fill { get; set; }
    public double Opacity { get; set; }
    public MapsHighlightSettingsBorder Border { get; set; }
}
```

### MapsTooltipSettings
Configures tooltip appearance.

```csharp
public class MapsTooltipSettings
{
    public bool Visible { get; set; }
    public string Template { get; set; }
    public TooltipGesture Gesture { get; set; }  // Click, Move
    public bool EnableAnimation { get; set; }
    public MapsTooltipBorder Border { get; set; }
    public MapsFontSettings TextStyle { get; set; }
}
```

### MapsMarkerSettings
Configures default marker appearance.

```csharp
public class MapsMarkerSettings
{
    public string Fill { get; set; }
    public double Opacity { get; set; }
    public double Width { get; set; }
    public double Height { get; set; }
    public MapsMarkerBorder Border { get; set; }
    public MarkerType Shape { get; set; }
    public MapsMarkerTooltipSettings TooltipSettings { get; set; }
    public MapsMarkerHighlightSettings HighlightSettings { get; set; }
    public MapsMarkerSelectionSettings SelectionSettings { get; set; }
}
```

### MapsMarkerClusterSettings
Configures marker clustering.

```csharp
public class MapsMarkerClusterSettings
{
    public bool AllowClustering { get; set; }
    public MarkerType Shape { get; set; }
    public double Width { get; set; }
    public double Height { get; set; }
    public string Fill { get; set; }
    public string LabelFill { get; set; }
    public MapsMarkerClusterBorder Border { get; set; }
    public int ClusterDistance { get; set; }
}
```

### MapsBubbleSettings
Configures bubble appearance.

```csharp
public class MapsBubbleSettings
{
    public string Fill { get; set; }
    public double MinRadius { get; set; }
    public double MaxRadius { get; set; }
    public double Opacity { get; set; }
    public MapsBubbleBorder Border { get; set; }
    public MapsBubbleColorMappings ColorMappings { get; set; }
    public MapsBubbleHighlightSettings HighlightSettings { get; set; }
    public MapsBubbleSelectionSettings SelectionSettings { get; set; }
}
```

### MapsShapeSettings
Configures shape/polygon appearance.

```csharp
public class MapsShapeSettings
{
    public string Fill { get; set; }
    public double Opacity { get; set; }
    public MapsShapeBorder Border { get; set; }
    public MapsShapeColorMappings ColorMappings { get; set; }
    public MapsLayerHighlightSettings HighlightSettings { get; set; }
    public MapsLayerSelectionSettings SelectionSettings { get; set; }
}
```

### MapsDataLabelSettings
Configures data labels on shapes.

```csharp
public class MapsDataLabelSettings
{
    public bool Visible { get; set; }
    public string LabelPath { get; set; }
    public SmartLabelMode SmartLabelMode { get; set; }  // None, Trim, Hide
    public IntersectAction IntersectAction { get; set; } // None, Trim, Wrap, Hide
    public MapsFontSettings TextStyle { get; set; }
}
```

### MapsAnnotation
Defines annotations (text, images, circles).

```csharp
public class MapsAnnotation
{
    public string Content { get; set; }
    public double X { get; set; }
    public double Y { get; set; }
    public double VerticalAlignment { get; set; }
    public double HorizontalAlignment { get; set; }
    public AnnotationAlignment Alignment { get; set; }
    public double ZIndex { get; set; }
}
```

### MapsNavigationLine
Defines navigation lines connecting locations.

```csharp
public class MapsNavigationLine
{
    public double[] Latitude { get; set; }
    public double[] Longitude { get; set; }
    public string Color { get; set; }
    public double Width { get; set; }
    public double Angle { get; set; }
    public string DashArray { get; set; }
    public bool Visible { get; set; }
    public double Opacity { get; set; }
    public MapsNavigationLineHighlightSettings HighlightSettings { get; set; }
    public MapsNavigationLineSelectionSettings SelectionSettings { get; set; }
}
```

### MapsPolygon
Defines polygon shapes.

```csharp
public class MapsPolygon
{
    public PolygonShapeType PolygonType { get; set; }
    public Coordinate[] Points { get; set; }
    public string Fill { get; set; }
    public double Opacity { get; set; }
    public MapsPolygonHighlightSettings HighlightSettings { get; set; }
    public MapsPolygonSelectionSettings SelectionSettings { get; set; }
}
```

---

## Event Arguments

### MarkerClickEventArgs
Triggered when marker is clicked.

```csharp
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

// Usage:
private void OnMarkerClick(MarkerClickEventArgs args)
{
    Console.WriteLine($"Marker clicked: ({args.Latitude}, {args.Longitude})");
}
```

### MarkerDragStartEventArgs
Triggered when marker drag starts.

```csharp
public class MarkerDragStartEventArgs
{
    public double Latitude { get; set; }
    public double Longitude { get; set; }
    public double X { get; internal set; }
    public double Y { get; internal set; }
    public int LayerIndex { get; internal set; }
    public int MarkerIndex { get; internal set; }
    public int DataIndex { get; internal set; }
}
```

### MarkerDragEndEventArgs
Triggered when marker drag ends.

```csharp
public class MarkerDragEndEventArgs
{
    public double Latitude { get; set; }
    public double Longitude { get; set; }
    public double X { get; internal set; }
    public double Y { get; internal set; }
    public int LayerIndex { get; internal set; }
    public int MarkerIndex { get; internal set; }
    public int DataIndex { get; internal set; }
    public Dictionary<string, object> Data { get; set; }
}
```

### MarkerMoveEventArgs
Triggered as marker moves (drag).

```csharp
public class MarkerMoveEventArgs
{
    public double Latitude { get; set; }
    public double Longitude { get; set; }
    public Dictionary<string, string> Data { get; set; }
    public string Target { get; set; }
    public double X { get; set; }
    public double Y { get; set; }
    public bool IsTouch { get; set; }
}
```

### BubbleClickEventArgs
Triggered when bubble is clicked.

```csharp
public class BubbleClickEventArgs
{
    public double Latitude { get; set; }
    public double Longitude { get; set; }
    public Dictionary<string, string> Data { get; set; }
    public double X { get; set; }
    public double Y { get; set; }
}
```

### ShapeSelectedEventArgs
Triggered when shape/polygon is selected.

```csharp
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

### SelectionEventArgs
Generic selection event.

```csharp
public class SelectionEventArgs
{
    public Dictionary<string, string> Data { get; set; }
    public string Fill { get; set; }
    public double Opacity { get; set; }
    public Dictionary<string, string> ShapeData { get; set; }
    public string Target { get; set; }
}
```

### MapZoomEventArgs
Triggered on zoom change.

```csharp
public class MapZoomEventArgs
{
    public double Scale { get; set; }
    public string Type { get; set; }  // "ZoomIn", "ZoomOut", or programmatic
    public PointF TileTranslatePoint { get; set; }
    public double TileZoomLevel { get; set; }
    public PointF TranslatePoint { get; set; }
}
```

### MapPanEventArgs
Triggered on pan.

```csharp
public class MapPanEventArgs
{
    public double Latitude { get; set; }
    public double Longitude { get; set; }
    public double Scale { get; set; }
    public PositionValues TileTranslatePoint { get; set; } = null!;
    public double TileZoomLevel { get; set; }
    public PositionValues TranslatePoint { get; set; } = null!;
}
```

### LabelRenderingEventArgs
Triggered before label rendering.

```csharp
public class LabelRenderingEventArgs
{
    public string Text { get; set; }
    public string Fill { get; set; }
    public int LayerIndex { get; set; }
    public double OffsetX { get; set; }
    public double OffsetY { get; set; }
}
```

### LayerRenderingEventArgs
Triggered before layer rendering.

```csharp
public class LayerRenderingEventArgs
{
    public double Index { get; set; }
    public bool Visible { get; set; }

}
```

### ShapeRenderingEventArgs
Triggered before shape rendering.

```csharp
public class ShapeRenderingEventArgs
{
    public Dictionary<string, string> Data { get; set; }
    public double Index { get; set; }
}
```

### MarkerRenderingEventArgs
Triggered before marker rendering.

```csharp
public class MarkerRenderingEventArgs
{
    public string ColorValuePath { get; set; }
    public string WidthValuePath { get; set; }
    public string HeightValuePath { get; set; }
    public Dictionary<string, string> Data { get; set; }
    public string Fill { get; set; }
    public double Height { get; set; }
    public string ImageUrl { get; set; }
    public string ImageUrlValuePath { get; set; }
    public MarkerType Shape { get; set; }
    public string ShapeValuePath { get; set; }
    public string Template { get; set; }
    public double Width { get; set; }
}
```

### BubbleRenderingEventArgs
Triggered before bubble rendering.

```csharp
public class BubbleRenderingEventArgs
{
    public Dictionary<string, string> Data { get; set; }
    public string Fill { get; set; }
    public double Radius { get; set; }
    public double CenterX { get; set; }
    public double CenterY { get; set; }
}
```

### AnimationCompleteEventArgs
Triggered when animation completes.

```csharp
public class AnimationCompleteEventArgs
{
    public DOM Element { get; set; }
}
```

### TooltipRenderEventArgs
Triggered before tooltip rendering.

```csharp
public class TooltipRenderEventArgs
{
    public Dictionary<string, string> Data { get; set; }
    public string Fill { get; set; }
    public string Content { get; set; }
}
```

### LegendRenderingEventArgs
Triggered before legend rendering.

```csharp
public class LegendRenderingEventArgs
{
    public string Text { get; set; }
    public string Fill { get; set; }
    public LegendShape Shape { get; set; }
}
```

### PrintEventArgs
Triggered before print.

```csharp
public class PrintEventArgs
{
    public bool Cancel { get; set; }
}
```

### ResizeEventArgs
Triggered on map resize.

```csharp
public class ResizeEventArgs
{
    public SizeF CurrentSize { get; set; }
    public SizeF PreviousSize { get; set; }
}
```

### LoadEventArgs
Triggered before map load.

```csharp
public class LoadEventArgs
{
    public bool Cancel { get; set; }
}
```

### LoadedEventArgs
Triggered after map load.

```csharp
public class LoadedEventArgs
{
    public object Maps { get; set; }
}
```

### AnnotationRenderingEventArgs
Triggered before annotation rendering.

```csharp
public class AnnotationRenderingEventArgs
{
    public bool Cancel { get; set; }
}
```

---

## Interfaces

### ILayer
Represents a layer in the map.

```csharp
public interface ILayer
{
    string Type { get; set; }
    object ShapeData { get; set; }
    object DataSource { get; set; }
}
```

### IMarker
Represents a marker.

```csharp
public interface IMarker
{
    double Latitude { get; set; }
    double Longitude { get; set; }
    MarkerType MarkerType { get; set; }
}
```

### IBubble
Represents a data bubble.

```csharp
public interface IBubble
{
    object Data { get; set; }
    double Radius { get; set; }
}
```

---

## Enums

### MarkerType
Defines marker shapes.

```csharp
public enum MarkerType
{
    Circle,      // Circular marker
    Rectangle,   // Square/rectangular marker
    Triangle,    // Triangular marker
    Cross,       // Plus/cross marker
    Diamond,     // Diamond-shaped marker
    Image,       // Custom image marker
    Balloon      // Balloon/callout shape
}
```

### ExportType
Defines export formats.

```csharp
public enum ExportType
{
    PNG,  // Export as PNG image
    SVG,  // Export as SVG vector
    PDF   // Export as PDF document
}
```

### ProjectionType
Defines map projections.

```csharp
public enum ProjectionType
{
    Mercator,             // Web Mercator (default)
    Winkel3,
    Miller,
    Eckert3,
    Eckert5,
    Eckert6,
    AitOff,
    Equirectangular
}
```

### GeometryType
Defines GeoJSON geometry types.

```csharp
public enum GeometryType
{
    Geographic,
    Normal
}
```

### LegendPosition
Defines legend placement.

```csharp
public enum LegendPosition
{
    Top,
    Bottom,
    Left,
    Right,    
    Float
}
```

### LegendMode
Defines legend rendering mode.

```csharp
public enum LegendMode
{
    Default,      // Static legend
    Interactive   // Interactive/togglable legend
}
```

### LegendType
Defines which elements are represented in legend.

```csharp
public enum LegendType
{
    Layers,
    Markers,
    Bubbles
}
```

### LegendArrangement
Defines legend item layout.

```csharp
public enum LegendArrangement
{
    Vertical,    // Items stacked vertically
    Horizontal   // Items arranged horizontally
}
```

### LegendShape
Defines shape used in legend items.

```csharp
public enum LegendShape
{
    Circle,
    Rectangle,
    Triangle,
    Diamond,
    Cross,
    Star,
    HorizontalLine,
    VerticalLine,
    Pentagon,
    Balloon,
    InvertedTriangle
}
```

### Alignment
Defines text/element alignment.

```csharp
public enum Alignment
{
    Near,    // Left/Top
    Center,
    Far      // Right/Bottom
}
```

### Orientation
Defines layout orientation.

```csharp
public enum Orientation
{
    Portrait,
    Landscape
}
```

### TooltipGesture
Defines interaction for tooltip display.

```csharp
public enum TooltipGesture
{
    Click,  // Show on click
    Move    // Show on hover/move
}
```

### BubbleType
Defines bubble sizing.

```csharp
public enum BubbleType
{
    Circle,     // Circular bubble
    Square      // Square bubble
}
```

### SmartLabelMode
Defines data label handling strategies.

```csharp
public enum SmartLabelMode
{
    None,     // No adjustment
    Trim,     // Truncate overflow text
    Hide,     // Hide labels that overlap
    Wrap      // Wrap text to multiple lines
}
```

### IntersectAction
Defines label intersection handling.

```csharp
public enum IntersectAction
{
    None,     // No adjustment
    Trim,     // Truncate text
    Wrap,     // Wrap to lines
    Hide      // Hide label
}
```

### LabelPosition
Defines label placement relative to legend.

```csharp
public enum LabelPosition
{
    Before,
    After
}
```

### ArrowPosition
Defines arrow placement on navigation line.

```csharp
public enum ArrowPosition
{
    Start,    // Arrow at line start
    End,      // Arrow at line end
    Both      // Arrows at both ends
}
```

### PolygonShapeType
Defines polygon shape types.

```csharp
public enum PolygonShapeType
{
    Polygon,
    LineString
}
```

### PanDirection
Defines pan direction.

```csharp
public enum PanDirection
{
    Left,
    Right,
    Top,
    Bottom,
    None
}
```

### ToolbarItem
Defines zoom toolbar buttons.

```csharp
public enum ToolbarItem
{
    ZoomIn,
    ZoomOut,
    Reset,
    Zoom,
    Pan
}
```

### Type
Defines layer data type.

```csharp
public enum Type
{
    Layer,
    SubLayer
}
```

---

## Properties Quick Reference

### Map Container Properties

| Property | Type | Purpose |
|----------|------|---------|
| `Width` | string | Container width (e.g., "100%", "800px") |
| `Height` | string | Container height (e.g., "600px", "100vh") |
| `Background` | string | Background color |
| `Margin` | MapsMargin | Map margins |
| `CenterPosition` | MapsCenterPosition | Initial center coordinates |
| `ZoomLevel` | int | Initial zoom (1-20) |
| `MinZoom` | double | Minimum zoom allowed |
| `MaxZoom` | double | Maximum zoom allowed |

### Interactive Properties

| Property | Type | Purpose |
|----------|------|---------|
| `EnableZoom` | bool | Allow zoom |
| `EnableZoomOnDoubleClick` | bool | Zoom on double-click |
| `EnablePinchZooming` | bool | Touch pinch zoom |
| `EnableDynamicZoom` | bool | Dynamic zoom interaction |
| `EnableNavigation` | bool | Show navigation controls |
| `EnableLegend` | bool | Show legend |
| `EnableSelection` | bool | Allow shape selection |
| `EnableHighlight` | bool | Hover highlight effect |

### Display Properties

| Property | Type | Purpose |
|----------|------|---------|
| `TitleSettings` | MapsTitleSettings | Main title |
| `SubtitleSettings` | MapsSubtitleSettings | Subtitle |
| `LegendSettings` | MapsLegendSettings | Legend configuration |
| `TooltipSettings` | MapsTooltipSettings | Tooltip settings |
| `AreaSettings` | MapsAreaSettings | Background/border |

### Marker Properties

| Property | Type | Purpose |
|----------|------|---------|
| `Fill` | string | Marker color |
| `Width` | double | Marker width |
| `Height` | double | Marker height |
| `BorderColor` | string | Border color |
| `BorderWidth` | double | Border width |
| `Opacity` | double | Transparency (0-1) |
| `Shape` | MarkerType | Shape type |
| `ImageUrl` | string | Custom image URL |

### Shape Properties

| Property | Type | Purpose |
|----------|------|---------|
| `Fill` | string | Shape color |
| `Stroke` | string | Border color |
| `StrokeWidth` | double | Border width |
| `Opacity` | double | Transparency |
| `ColorValuePath` | string | Data field for color mapping |
| `ValuePath` | string | Data field for values |

---

For complete API documentation, visit the [Official Syncfusion Blazor Maps API Reference](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.Maps.html)
